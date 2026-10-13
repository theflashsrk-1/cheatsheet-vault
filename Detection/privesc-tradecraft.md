#detection #privesc #uac-bypass #token-impersonation #potato-attacks

# Privilege Escalation Tradecraft Analysis (Detection Mindset)

Related: [exploits](../AD/exploits.md) · [privesc](../Windows/privesc.md) · [token-manipulation](token-manipulation.md) · [kernel-telemetry](kernel-telemetry.md)

## UAC Bypass Detection Logic
- **Core signal**: a Medium-integrity parent spawning a High-integrity child outside of a normal consent prompt. Correlate by `process_guid`, not PID (PIDs are reused).
- **Auto-elevating binaries**: `fodhelper.exe`, `sdclt.exe`, `computerdefaults.exe` (listed in `AutoApproveEXEList`) auto-elevate for admins — treat unexpected children (`cmd.exe`, `powershell.exe`) as bypass indicators.
- **Elevated COM**: COM classes marked elevated run under `dllhost.exe /ProcessId:{CLSID}`; map CLSIDs (e.g. `ICMLuaUtil`) to flag unusual activations.
- **Registry hijack**: writes to `HKCU\Software\Classes\ms-settings\Shell\Open\command` + `DelegateExecute` (the fodhelper/ms-settings bypass) are suspicious coming from a non-admin user process.
- **DLL hijacking**: watch for unsigned DLLs loaded alongside signed Microsoft DLLs in the same (especially auto-elevated) process. UACME-documented DLL names used in these chains: `DismCore.dll`, `omctl32.dll`, `osksupport.dll`.
- **Trusted-path collision**: directories with trailing spaces (e.g. `C:\Windows `, `C:\Program Files `) can be misparsed as the real system path (`GetLongPathNameW` normalization bug) — EXE/DLL creation there is highly suspicious.

### Relevant Event Sources
| Source | Event ID | Use |
|---|---|---|
| Sysmon | 1 | Process creation — integrity transitions, auto-approved parent → odd child |
| Sysmon | 7 | Image loaded — unsigned DLL / known UACME DLL names |
| Sysmon | 11 | FileCreate — EXE/DLL dropped in rogue system-like paths |
| Sysmon | 12/13/14 | Registry create/modify/delete — `ms-settings\Shell\Open\command` writes |
| ETW | `Microsoft-Windows-LUA-ConsentUI/Diagnostic` EventID 16001 | Executable elevation attempts |
| ETW | `Microsoft-Windows-LUA-ConsentUI/Diagnostic` EventID 16005 | Elevated COM class activations |

## Access Token / Impersonation Attacks
- `SeDebugPrivilege` is typically enabled first, to open `winlogon.exe` (runs as SYSTEM) and grab its primary token via `OpenProcessToken`.
- `ImpersonateLoggedOnUser` internally: `NtQueryInformationToken` → `NtDuplicateToken` (primary → impersonation token) → `NtSetInformationThread`.
- After impersonation, `OpenThreadToken` on the current thread returns the new impersonation token — a thread can run as SYSTEM while the process itself stays as the original user.
- WinDbg: `!process` shows a process's primary token address; `!token` inspects it (SID, groups, privileges, `TokenType`, `AuthenticationId`, `ModifiedId`). `_ETHREAD->ClientSecurity` (`_PS_CLIENT_SECURITY_CONTEXT`) holds per-thread impersonation state.
- **Defender note**: impersonation is invisible at the process level — only the thread carries SYSTEM. Monitor token operations and access to SYSTEM-owned processes, not just process identity.

## Service-Account → SYSTEM Escalation (Potato-family)
- **Core telemetry**: Sysmon EventCode 1 (process create), 3 (network connect), 10 (process access), 17/18 (pipe create/connect); Zeek DCE/RPC; Security log RPC auditing; ETW.
- **SYSTEM process with non-SYSTEM parent**: `EventCode=1` where `user=SYSTEM AND ParentUser != "NT AUTHORITY\SYSTEM"` — e.g. `LOCAL SERVICE` spawning a SYSTEM `cmd.exe` (SweetPotato-style).
- **JuicyPotato**: relays an NTLM auth from SYSTEM to `RPCSS` on local port 135 — watch for unusual local NTLM/anonymous-logon traffic to 135. Only reliable pre-1809 / pre-Server 2019.
- **Named-pipe abuse** (RoguePotato / PrintSpoofer / PetitPotam): `EventCode=17` = pipe created (server), `EventCode=18` = pipe connected (client). Pattern: non-SYSTEM creates pipe, SYSTEM connects, same `PipeName` — e.g. `\pipe\srvsvc`, `\pipe\spoolss`, `\pipe\epmapper`.
- **RoguePotato**: stands up a fake OXID resolver; look for `IObjectExporter`/`ResolveOxid2` calls (Zeek DCE/RPC) to non-standard external endpoints. Internally routes through `CoGetInstanceFromIStorage` / `IStandardActivator::StandardGetInstanceFromIStorage` → `DoGetInstanceFromStorage` — monitor both entry points.
- **RPC coercion** (EfsRpc/Print Spooler): `EfsRpcOpenFileRaw`, `EfsRpcEncryptFileSrv`, `EfsRpcQueryUsersOnFile`, `RpcRemoteFindFirstPrinterChangeNotificationEx` force SYSTEM/DC authentication; often over local RPC (LPC), so network logs alone miss it.
```cmd
netsh rpc filter add filter
```
Register an RPC endpoint-auditing filter (e.g. on the EfsRpc interface UUID) to log access attempts into the Security log.

- **RPCSS token theft** (RoguePotato): source process impersonates NETWORK SERVICE then accesses `RPCSS` (`svchost.exe -k RPCSS -p`) to get a SYSTEM token. Detect via Sysmon `EventCode=10` where `TargetUser="NT AUTHORITY\NETWORK SERVICE"` and source isn't SYSTEM, joined with `EventCode=1` for the RPCSS process.
- **Rogue WinRM listener** (RogueWinRM): stops the real WinRM service to free TCP 5985, restarts BITS to trigger local auth to the rogue listener, nets SYSTEM. Detect via Sysmon `EventCode=3, dest_port=5985, Initiated=false` and correlate the `<unknown process>` image with `EventCode=1` by PID.

## Privileges Commonly Abused
| Privilege | Impact |
|---|---|
| `SeImpersonatePrivilege` | Adopt an authenticated client's token (core of Potato attacks) |
| `SeAssignPrimaryTokenPrivilege` | Assign a different primary token to a process |
| `SeCreateTokenPrivilege` | Forge arbitrary tokens (including SYSTEM/domain admin SIDs) |
| `SeDebugPrivilege` | Open/control any process, incl. SYSTEM-owned, for token theft |
| `SeLoadDriverPrivilege` | Load a (vulnerable) driver for kernel-level token manipulation |

See [exploits](../AD/exploits.md) for the AD-side privilege-escalation path and [privesc](../Windows/privesc.md) for local enumeration.
