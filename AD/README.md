# AD (Active Directory)

Enumeration, Kerberos abuse, ACL/DACL attacks, trust abuse, relaying, AD CS, and the tooling (NetExec) that ties it all together — the core of any internal AD-focused engagement.

| Note | Covers |
|---|---|
| [enumeration](enumeration.md) | Unauthenticated/authenticated AD enumeration basics, RSAT, PowerView/SharpView, BloodHound collection |
| [credentialed-enumeration](credentialed-enumeration.md) | Linux-side AD enumeration tooling (RDP/PsExec/WMIExec/WinRM access, SMB shares, RPC, BloodHound) |
| [powershell-management](powershell-management.md) | AD management via native PowerShell/RSAT — users, groups, OUs, GPOs, computers |
| [password-spraying](password-spraying.md) | Password spraying & LLMNR/NBT-NS poisoning |
| [kerberoasting](kerberoasting.md) | Kerberoasting & AS-REP roasting |
| [kerberos-tickets](kerberos-tickets.md) | Golden/silver ticket attacks, pass-the-ticket, overpass-the-hash |
| [kerberos-delegation](kerberos-delegation.md) | Unconstrained/constrained/RBCD delegation abuse |
| [dacl-attacks](dacl-attacks.md) | DACL/ACL abuse (GenericAll, WriteDACL, shadow credentials, GPO abuse, sAMAccountName spoofing) |
| [dcsync](dcsync.md) | DCSync credential extraction |
| [ntlm-relay](ntlm-relay.md) | NTLM relay attacks (coercion, ntlmrelayx, AD CS relay) |
| [trust-attacks](trust-attacks.md) | Cross-domain/forest trust attacks (ExtraSids, SIDHistory, GoldenGMSA) |
| [mssql-sccm](mssql-sccm.md) | MSSQL, Exchange & SCCM attack paths |
| [NetExec](NetExec.md) | NetExec (`nxc`) usage — the maintained successor to CrackMapExec |
| [exploits](exploits.md) | AD CVE exploits (NoPac, PrintNightmare, PetitPotam) |
| [adcs](adcs.md) | AD CS abuse, full ESC1–ESC13 chain (Certify/Certipy) |
