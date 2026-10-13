# Windows

Privilege escalation, lateral movement, credential theft, persistence, and low-level process-injection tradecraft on Windows hosts — standalone notes, cross-linked to [AD](../AD) for domain-specific attacks.

## Notes

| Note | Covers |
|---|---|
| [fundamentals](fundamentals.md) | OS info, file/service permissions, PowerShell basics |
| [command-line](command-line.md) | Built-in CMD/PowerShell administration reference (users, services, tasks, remote exec) |
| [credential-theft](credential-theft.md) | Hunting stored secrets — file content search, stored credentials, LaZagne/Mimikatz |
| [privesc](privesc.md) | Privilege escalation — enumeration, Potato attacks, LSASS dump, service abuse, AlwaysInstallElevated |
| [lateral-movement](lateral-movement.md) | Moving between hosts via RDP, pass-the-hash/ticket, SMB (PsExec), WMI, WinRM, DCOM, SSH, VNC |
| [attacks-defense](attacks-defense.md) | End-to-end AD attack chain (Kerberoast, GPP passwords, DCSync, delegation, ADCS) with defensive notes |
| [process-injection](process-injection.md) | Shellcode/payload generation, injection techniques (classic, APC, thread hijack, hollowing), WinDbg inspection |
| [low-level-detectability](low-level-detectability.md) | PE file format internals and malware-relevant Win32 API surface for detection/evasion |
| [persistence](persistence.md) | Run keys, scheduled tasks, services, WMI event subscriptions, DLL/COM hijacking, GPO logon scripts |
