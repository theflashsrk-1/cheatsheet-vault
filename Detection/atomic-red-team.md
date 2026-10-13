#detection #atomic-red-team #splunk #wazuh #threat-hunting

# Atomic Red Team & Detection Queries

Related: [kernel-telemetry](kernel-telemetry.md) · [privesc-tradecraft](privesc-tradecraft.md) · [siem](siem.md)

## Contents
- [Hunting Query Index](#hunting-query-index)
- [Atomic Red Team (PowerShell)](#atomic-red-team-powershell)
- [Splunk (SPL) Hunting Queries](#splunk-spl-hunting-queries)
- [Wazuh (WQL) Hunting Queries](#wazuh-wql-hunting-queries)

## Hunting Query Index
Quick map of technique → tool → what the query below catches, before the full queries.

| Technique | Platform | Detects |
|---|---|---|
| T1014 (Rootkit) | Splunk / auditd | Writes to `/etc/ld.so.preload` (dynamic-linker hijack) |
| T1485 (Data Destruction) | Splunk / Sysmon-for-Linux | `dd` raw disk/file overwrite |
| T1136.001 (Local Account Creation) | Splunk / Windows Security | EventCode 4720 (local account created) |
| — (Internal recon) | Splunk / Zeek `conn.log` | Internal port-scanning (many dest ports/src-dst pair in 5m) |
| T1014 (Rootkit, cron variant) | Wazuh / Sysmon-for-Linux | File create under `/etc/cron.d/` |
| T1547.001 (Registry Run Keys) | Wazuh / Sysmon | `reg.exe` execution |
| — (Credential access) | Wazuh / Sysmon | Load of `vaultcli.dll` (Windows Vault access) |

## Atomic Red Team (PowerShell)
```powershell
Import-Module ~/AtomicRedTeam/invoke-atomicredteam/Invoke-AtomicRedTeam.psd1
```
Load the Atomic Red Team execution module.

```powershell
Invoke-AtomicTest T1014 -TestNumbers 3 -ShowDetails
```
Show details of a specific test (by MITRE technique ID + test number) without running it.

```powershell
Invoke-AtomicTest T1014 -TestNumbers 3
```
Execute that atomic test (here: a rootkit-simulation test for T1014).

<details>
<summary><strong>Splunk (SPL) Hunting Queries</strong> — by technique (T1014, T1485, T1136.001, internal recon)</summary>

## Splunk (SPL) Hunting Queries

**T1014 — Rootkit (dynamic-linker persistence)**
```spl
index=main host="ubuntu" source="auditd" type=PATH name="/etc/ld.so.preload"
```
Detect dynamic-linker-based rootkit persistence (T1014) via `/etc/ld.so.preload` writes.

**T1485 — Data Destruction**
```spl
index=main host="ubuntu" source="journald://sysmon" EventCode=1 Image="*dd" CommandLine="*of=*"
```
Detect `dd`-based raw disk/file overwrite (T1485) on Linux/macOS/BSD.

**T1136.001 — Create Local Account**
```spl
index="main" host="LOGGING-VM" source=WinEventLog:Security EventCode=4720 | stats count by host, Subject_Account_Name, New_Account_Account_Name, New_Account_Domain, Primary_Group_ID, status, _time
```
Detect local account creation (T1136.001) via Windows Security EventCode 4720.

**Internal port-scan recon**
```spl
index=zeek source="/opt/zeek/logs/current/conn.log" orig_bytes=0 dest_ip IN (192.168.0.0/16, 172.16.0.0/12, 10.0.0.0/8) | bin span=5m _time | stats dc(dest_port) as num_dest_port by _time, src_ip, dest_ip | where num_dest_port >= 3
```
Detect internal port-scanning behavior from Zeek `conn.log`.

</details>

<details>
<summary><strong>Wazuh (WQL) Hunting Queries</strong> — by technique (T1014/cron, T1547.001, credential access)</summary>

## Wazuh (WQL) Hunting Queries

**T1014 — Rootkit (cron-based persistence variant)**
```text
agent.name:"LINUX-LOGGING" AND data.system.channel:"Linux-Sysmon/Operational" AND data.system.eventId:11 AND data.eventdata.targetFilename:*/etc/cron.d/*
```
Detect file creation/write under `/etc/cron.d/` (cron-based persistence) via Sysmon-for-Linux.

**T1547.001 — Registry Run Keys / Startup Folder**
```text
agent.name:"Logging-VM" AND data.win.system.channel:"Microsoft-Windows-Sysmon/Operational" AND data.win.eventdata.image:*reg.exe
```
Detect `reg.exe` usage — commonly tied to Run-key persistence (T1547.001).

**Credential access — Windows Vault**
```text
agent.name:"Logging-VM" AND data.win.system.eventID:7 AND data.win.eventdata.originalFileName:"vaultcli.dll"
```
Detect loading of `vaultcli.dll` (Windows Vault / credential-store access) via Sysmon Event ID 7.

</details>
