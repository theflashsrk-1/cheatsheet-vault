# Detection / Blue Team

Blue-team side of the vault — SIEM queries, forensic artifacts, and kernel-level telemetry for spotting the exact tradecraft the offensive notes describe.

| Note | Covers |
|---|---|
| [siem](siem.md) | SIEM fundamentals (KQL/Elastic) — logon monitoring, group changes, LOLBin detection, event ID reference |
| [linux-forensics](linux-forensics.md) | Linux logs, auditd, journald, and memory forensics |
| [user-behavior-forensics](user-behavior-forensics.md) | Windows DFIR registry artifacts — shellbags, UserAssist, RecentDocs, USBSTOR |
| [malicious-docs](malicious-docs.md) | PDF/Office/RTF malicious document triage and shellcode emulation |
| [evasion](evasion.md) | Offensive AMSI/UAC/AppLocker/CLM bypass techniques, from a detection angle |
| [token-manipulation](token-manipulation.md) | Detecting access token manipulation via session enumeration and kernel structure inspection |
| [kernel-telemetry](kernel-telemetry.md) | Windows kernel telemetry via ETW/WFP providers |
| [privesc-tradecraft](privesc-tradecraft.md) | Detection logic for privilege-escalation tradecraft — UAC bypass, token impersonation, Potato attacks |
| [atomic-red-team](atomic-red-team.md) | Atomic Red Team technique execution plus matching Splunk (SPL) and Wazuh (WQL) hunting queries |
