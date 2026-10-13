# Tools

C2 frameworks, shell/payload generation, file transfer tricks, and the memory-forensics workflow used to pull creds without touching LSASS live. Every tool referenced anywhere in this vault has its install command in **[tool-index.md](tool-index.md)** — start there to get a box set up.

| Note | Covers |
|---|---|
| [tool-index](tool-index.md) | Install command for every tool in this vault, grouped by category (AD/Windows, Web, Network, Cloud, Containers, Mobile, Reversing, Detection/Forensics, Wireless, Secrets/OSINT) |
| [shells-payloads](shells-payloads.md) | Netcat listeners, PowerShell reverse-shell one-liner, msfvenom payloads per OS/format (ELF, EXE, ASP, WAR/JSP, Mach-O), MS17-010, TTY upgrades |
| [metasploit](metasploit.md) | msfconsole module workflow, session management, Meterpreter post-exploitation commands |
| [sliver](sliver.md) | Sliver C2 listeners/profiles, implant generation, session & beacon management, post-exploitation, armory extensions |
| [file-transfers](file-transfers.md) | Windows/Linux download & upload one-liners (PowerShell, BITS, certutil, wget, PHP), SCP/rsync/sftp, quick HTTP/SMB file serving |
| [memory-forensics](memory-forensics.md) | winpmem/DumpIt memory acquisition + Volatility 2/3 analysis — EDR-evasive credential extraction that never touches `lsass.exe` live |

---

*For authorized security testing, CTF competitions, and defensive research only.*
