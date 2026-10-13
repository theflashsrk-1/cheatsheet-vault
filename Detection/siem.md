#detection #siem #kql #elastic #blue-team

# SIEM Fundamentals (KQL / Elastic)

Detection queries for Windows event logs. Related: [kernel-telemetry](kernel-telemetry.md) · [evasion](evasion.md) · [lateral-movement](../Windows/lateral-movement.md)

## Contents
- [Logon Monitoring](#logon-monitoring)
- [Group Changes](#group-changes)
- [Noise Reduction](#noise-reduction)
- [LOLBin Detection](#lolbin-detection)
- [Windows Event ID Reference](#windows-event-id-reference)
- [Field Reference](#field-reference)

## Logon Monitoring
```kql
event.code:4625
```
Failed logons (4624 = success).

```kql
event.code:4625 AND winlog.event_data.SubStatus:0xC0000072
```
Failed logons against disabled accounts.

```kql
event.code:4625 AND user.name: admin*
```
Failed logons for usernames starting "admin".

## Group Changes
```kql
(event.code:4732 OR event.code:4733) AND group.name:administrators
```
Additions/removals to the local Administrators group.

## Noise Reduction
```kql
NOT user.name: *$ AND winlog.channel.keyword: Security
```
Exclude computer accounts; keep only the Security channel.

## LOLBin Detection
```kql
process.name:MSBuild.exe AND process.parent.name:(excel.exe OR winword.exe)
```
MSBuild spawned by Office — living-off-the-land abuse. See [evasion](evasion.md).

## Windows Event ID Reference
Quick-reference for the Event IDs used in the queries above:

| Event ID | Meaning | Query |
|---|---|---|
| 4624 | Successful logon | — |
| 4625 | Failed logon | `event.code:4625` |
| 4625 (SubStatus `0xC0000072`) | Failed logon — account disabled | `event.code:4625 AND winlog.event_data.SubStatus:0xC0000072` |
| 4732 | Member added to a local group | `(event.code:4732 OR event.code:4733) AND group.name:administrators` |
| 4733 | Member removed from a local group | `(event.code:4732 OR event.code:4733) AND group.name:administrators` |

## Field Reference
```text
.keyword  -> exact match / aggregations
(no suffix) -> wildcard searches in KQL
@timestamp, user.name, host.hostname, process.parent.name, winlog.logon.type
```
Use `.keyword` for exact/aggregation, bare field for wildcard.
