#detection #tokens #windbg #privileges

# Detecting Access Token Manipulation

Token enumeration + kernel structure inspection. Related: [kernel-telemetry](kernel-telemetry.md) · [privesc](../Windows/privesc.md) · [windbg](../Reversing/windbg.md)

## Enumerate Sessions & Tokens
```cmd
C:\Tools\SysinternalsSuite\logonsessions64.exe -p
```
List processes per logon session.

```cmd
C:\Tools\Tokenvator.exe
Enable_Privilege /Privilege:SeBackupPrivilege
```
Enable privileges on a token (Tokenvator).

## NTObjectManager (PowerShell)
```powershell
Install-Module NTObjectManager
```
Install the module.

```powershell
Get-NtToken -ProcessId 2144
```
Retrieve a process's token (`Get-NtTokenPrivilege`, `Show-NtToken -All`).

## WinDbg Kernel Inspection
```
dt nt!_EPROCESS
```
Show the EPROCESS structure.

```
dt nt!_PS_PROTECTION
```
Inspect protected-process fields (`_PS_PROTECTED_TYPE`, `_PS_PROTECTED_SIGNER`).

```
!process 0 0 <process>.exe
```
Inspect a running process (`dt nt!_SECURITY_DESCRIPTOR` for ACLs). See [windbg](../Reversing/windbg.md).
