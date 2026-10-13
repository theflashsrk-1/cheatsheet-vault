#detection #etw #windbg #kernel #windows

# Windows Kernel Telemetry & Detection (ETW / WFP)

Related: [process-injection](../Windows/process-injection.md) · [low-level-detectability](../Windows/low-level-detectability.md) · [privesc-tradecraft](privesc-tradecraft.md)

## ETW Providers (PowerShell / logman)
```powershell
logman query providers
logman query providers | findstr /I "Microsoft-Windows-Kernel-Process"
logman query providers Microsoft-Windows-Threat-Intelligence
logman query providers -pid 3632
logman query -ets
logman query "EventLog-System" -ets
```
List all ETW providers, filter by name, inspect a specific provider's GUID/keywords, list providers a given PID can emit to, and list/inspect active Event Trace Sessions.

## Mimikatz Kernel Driver (Notification Callbacks)
```text
mimikatz # !+
mimikatz # !notifProcess
mimikatz # !notifThread
mimikatz # !notifImage
mimikatz # !notifReg
```
Load mimikatz's kernel driver and dump registered process/thread/image-load/registry notify-callback routines. See [exploits](../AD/exploits.md).

## WFP Inspection (WTF-WFP PowerShell Module)
```powershell
Install-Module -Name NtObjectManager
Install-Module wtf-wfp
Get-FwLayer
Get-FwSubLayer | Sort-Object Weight -Desc | Select-Object KeyName,Weight
Get-FwFilter
Get-FwFilter | Group-Object -Property SubLayerKeyName | Select-Object Name,Count
Get-FwFilter | Group-Object -Property LayerKeyName | Select-Object Name,Count
Get-FwFilter | Group-Object -Property ActionType | Select-Object Name,Count
Get-FwCallout
```
Enumerate Windows Filtering Platform layers, sublayers, filters (grouped by sublayer/layer/action), and registered callouts.

## WinDbg Kernel Commands (ETW-TI / Process Internals)
```windbg
!wmitrace.strdump
dx &((nt!_WMI_LOGGER_CONTEXT*)0xADDR)->Consumers
dx @$consumers = Debugger.Utility.Collections.FromListEntry(*(nt!_LIST_ENTRY*)0xADDR, "nt!_ETW_REALTIME_CONSUMER", "Links")
dx @$consumers.Select(p => (char*)p.ProcessObject->ImageFileName)
dt _ETW_REG_ENTRY poi(nt!EtwThreatIntProvReghandle)
dx (*(_ETW_REG_ENTRY**)&nt!EtwKernelProvRegHandle)->GuidEntry->Guid
bp nt!EtwTiLogAllocExecVm ".printf \"Executable Memory Alloc at 0x%p from %ma\\n\", @r8, @rcx+0x5a8;g"
dx ((_EPROCESS*)0xADDR)->Protection
dx ((_EPROCESS*)0xADDR)->VadRoot
!vad 0xADDR
dx ((_MMVAD*)0xADDR)->Core->u->VadFlags
dx ((void**[0x40])&nt!PspCreateProcessNotifyRoutine).Where(c => c != 0).Select(c => *(void**)((c & ~0xF) + 8))
rdmsr 0xc0000082
!drvobj \FileSystem\Ntfs 7
!fltkd.filters
```

Dump ETW logger session consumers, resolve the ETW-TI kernel provider's GUID/handle, set a breakpoint logging every executable memory allocation (`EtwTiLogAllocExecVm`), inspect a process's protection level and VAD tree, enumerate process-creation notify routines, read the `IA32_LSTAR` MSR (syscall entry point), and list minifilter/filesystem driver major functions.

## ETW-TI Keywords (Microsoft-Windows-Threat-Intelligence)
Key flags (hex): `ALLOCVM_LOCAL` `0x1`, `ALLOCVM_REMOTE` `0x4`, `PROTECTVM_LOCAL` `0x10`, `PROTECTVM_REMOTE` `0x40`, `MAPVIEW_LOCAL` `0x100`, `MAPVIEW_REMOTE` `0x400`, `QUEUEUSERAPC_REMOTE` `0x1000`, `SETTHREADCONTEXT_REMOTE` `0x4000`, `READVM_LOCAL`/`REMOTE` `0x10000`/`0x20000`, `WRITEVM_LOCAL`/`REMOTE` `0x40000`/`0x80000`, `SUSPEND_THREAD` `0x100000`, `SUSPEND_PROCESS` `0x400000`, `DRIVER_EVENTS` `0x40000000`, `DEVICE_EVENTS` `0x80000000`. Each pairs a "LOCAL" variant with a "REMOTE" and "_KERNEL_CALLER" variant — these back Sysmon/EDR detection of [process-injection](../Windows/process-injection.md) primitives (`VirtualAllocEx`, `WriteProcessMemory`, `QueueUserAPC`, `SetThreadContext`, etc.).

## Registry Callback Operations
`CreateKey` `OpenKey` `DeleteKey` `SetValueKey` `DeleteValueKey` `SetInformationKey` `RenameKey` `EnumerateKey` `EnumerateValueKey` `QueryKey` `QueryValueKey` `QueryMultipleValueKey` `FlushKey` `LoadKey` `UnLoadKey` `KeyHandleClose` — operations exposed to `CmRegisterCallbackEx` kernel registry filters.

## Sysmon Event IDs
| ID | Meaning |
|---|---|
| 1 | Process creation |
| 3 | Network connection |
| 7 | Image loaded |
| 8 | CreateRemoteThread detected |
| 10 | Process access via OpenProcess |
| 11 | File creation (or timestamp change) |
| 13 | Registry key modification |
| 17 | Pipe created |
