<div align="center">

# 🗂️ Pentest Cheat Sheet Vault

**A command-verified, cross-linked Obsidian vault for offensive security engagements.**

![Notes](https://img.shields.io/badge/notes-100%2B-blue)
![Topics](https://img.shields.io/badge/topics-14-informational)
![Format](https://img.shields.io/badge/format-Obsidian%20Markdown-7c3aed)
![License](https://img.shields.io/badge/use-authorized%20testing%20only-critical)

</div>

---

Terse, copy-pasteable commands with one-line descriptions — no filler. Built from the [HackTheBox-All-Cheatsheets](https://github.com/Fr6ey/HackTheBox-All-Cheatsheets) module library, then extended with full coverage for cloud, containers, binary exploitation, supply-chain/CI-CD, and more that the source material didn't touch.

### Using it
- **Obsidian**: open this repo's folder as a vault — all `[[wikilinks]]` resolve and the graph view shows how everything connects.
- **Plain Markdown / GitHub**: every note stands on its own; just browse the folders below.
- Every tool referenced anywhere in here has an install command in **[Tools/tool-index.md](Tools/tool-index.md)** — one stop for getting your box set up.

---

## Contents

| Folder | Covers |
|---|---|
| [AD](#ad--active-directory) | Kerberos, DACLs, trusts, relaying, ADCS, NetExec |
| [Windows](#windows) | Privesc, lateral movement, credential theft, persistence, injection |
| [Linux](#linux) | Privesc, fundamentals, ELF/process-injection detection |
| [Cloud](#cloud) | AWS / Azure / GCP enumeration & privesc |
| [Containers](#containers) | Docker escape, Kubernetes RBAC abuse |
| [Wireless](#wireless-bluetooth--rfid--nfc) | Bluetooth LE, RFID/NFC (Proxmark3) |
| [Web](#web) | Full injection/auth/logic attack surface |
| [WiFi](#wifi) | WEP → WPA3, evil twin, captive portals |
| [Network](#network) | Recon, pivoting, subdomain enum |
| [Passwords](#passwords) | Cracking, spraying, brute-forcing |
| [Mobile](#mobile) | Android + iOS static/dynamic analysis |
| [OSINT](#osint) | Corporate recon |
| [Reversing](#reversing) | Assembly, Windows/Linux binary exploitation |
| [Detection](#detection--blue-team) | Blue-team side: SIEM, forensics, ETW, hunting |
| [CodeReview](#codereview-whitebox) | Whitebox review, supply chain, CI/CD |
| [Tools](#tools) | C2, shells, memory forensics, the tool index |

---

## AD (Active Directory)
- [enumeration](AD/enumeration.md) — unauthenticated/authenticated AD enumeration basics
- [credentialed-enumeration](AD/credentialed-enumeration.md) — Linux-side AD enumeration tooling
- [powershell-management](AD/powershell-management.md) — AD management via native PowerShell/RSAT
- [password-spraying](AD/password-spraying.md) — password spraying & LLMNR/NBT-NS poisoning
- [kerberoasting](AD/kerberoasting.md) — Kerberoasting & AS-REP roasting
- [kerberos-tickets](AD/kerberos-tickets.md) — golden/silver/diamond ticket attacks
- [kerberos-delegation](AD/kerberos-delegation.md) — unconstrained/constrained/RBCD delegation abuse
- [dacl-attacks](AD/dacl-attacks.md) — DACL/ACL abuse (GenericAll, WriteDACL, etc.)
- [dcsync](AD/dcsync.md) — DCSync credential extraction
- [ntlm-relay](AD/ntlm-relay.md) — NTLM relay attacks
- [trust-attacks](AD/trust-attacks.md) — cross-domain/forest trust attacks
- [mssql-sccm](AD/mssql-sccm.md) — MSSQL, Exchange & SCCM attack paths
- [netexec](AD/netexec.md) — NetExec (`nxc`) usage
- [exploits](AD/exploits.md) — AD CVE exploits (noPac, PrintNightmare, PetitPotam)
- [adcs](AD/adcs.md) — AD CS abuse, full ESC1–ESC13 chain (Certify/Certipy)

## Windows
- [fundamentals](Windows/fundamentals.md) · [command-line](Windows/command-line.md) · [credential-theft](Windows/credential-theft.md)
- [privesc](Windows/privesc.md) · [lateral-movement](Windows/lateral-movement.md) · [attacks-defense](Windows/attacks-defense.md)
- [process-injection](Windows/process-injection.md) · [low-level-detectability](Windows/low-level-detectability.md)
- [persistence](Windows/persistence.md) — Run keys, scheduled tasks, services, WMI, COM hijacking

## Linux
- [fundamentals](Linux/fundamentals.md) · [privesc](Linux/privesc.md) · [macos](Linux/macos.md)
- [process-injection-detection](Linux/process-injection-detection.md) — ELF internals, gdb/ptrace, auditctl

## Cloud
- [aws](Cloud/aws.md) — IMDS SSRF, IAM privesc, Pacu/CloudFox/ScoutSuite
- [azure](Cloud/azure.md) — Entra ID enum, password spraying, ROADrecon/AzureHound
- [gcp](Cloud/gcp.md) — IMDS SSRF, service-account privesc, ScoutSuite

## Containers
- [docker](Containers/docker.md) — enumeration & escape (socket, privileged, capabilities)
- [kubernetes](Containers/kubernetes.md) — kubectl enum, RBAC abuse, pod→node escape

## Wireless (Bluetooth / RFID / NFC)
- [bluetooth-rfid](Wireless/bluetooth-rfid.md) — BLE GATT abuse, Proxmark3 (MIFARE/HID)

## Web
- [web-recon](Web/web-recon.md) · [web-requests](Web/web-requests.md) · [web-proxies](Web/web-proxies.md)
- [fuzzing](Web/fuzzing.md) · [ffuf](Web/ffuf.md)
- [sqli](Web/sqli.md) · [sqli-advanced](Web/sqli-advanced.md) · [blind-sqli](Web/blind-sqli.md) · [sqlmap](Web/sqlmap.md) · [nosqli](Web/nosqli.md)
- [xss](Web/xss.md) · [csrf-cors-csp](Web/csrf-cors-csp.md) · [auth-attacks](Web/auth-attacks.md)
- [file-inclusion](Web/file-inclusion.md) · [file-upload](Web/file-upload.md) · [command-injection](Web/command-injection.md) · [injection-attacks](Web/injection-attacks.md)
- [deserialization](Web/deserialization.md) · [server-side-attacks](Web/server-side-attacks.md) · [http-attacks](Web/http-attacks.md) · [http-misconfigurations](Web/http-misconfigurations.md) · [tls-attacks](Web/tls-attacks.md)
- [graphql](Web/graphql.md) · [web-attacks](Web/web-attacks.md) · [common-applications](Web/common-applications.md) · [wordpress](Web/wordpress.md)
- [business-logic](Web/business-logic.md) — race conditions, IDOR/BOLA, mass assignment, workflow bypass

## WiFi
- [basics](WiFi/basics.md) · [tools](WiFi/tools.md) · [wep](WiFi/wep.md)
- [wpa-wpa2](WiFi/wpa-wpa2.md) · [wpa3](WiFi/wpa3.md) · [wps](WiFi/wps.md)
- [evil-twin](WiFi/evil-twin.md) · [captive-portals](WiFi/captive-portals.md) · [password-cracking](WiFi/password-cracking.md)

## Network
- [footprinting](Network/footprinting.md) · [nmap](Network/nmap.md) · [common-services](Network/common-services.md)
- [pivoting](Network/pivoting.md) · [traffic-analysis](Network/traffic-analysis.md)
- [subdomain-enum](Network/subdomain-enum.md) — subfinder/amass/assetfinder, subdomain takeover

## Passwords
- [password-attacks](Passwords/password-attacks.md) · [login-brute-forcing](Passwords/login-brute-forcing.md) · [hashcat](Passwords/hashcat.md)

## Mobile
- [android-fundamentals](Mobile/android-fundamentals.md) · [static-analysis](Mobile/static-analysis.md) · [dynamic-analysis](Mobile/dynamic-analysis.md)
- [malware-analysis](Mobile/malware-analysis.md) · [automation](Mobile/automation.md)
- [ios](Mobile/ios.md) — IPA static analysis, Frida/Objection, Keychain

## OSINT
- [corporate-recon](OSINT/corporate-recon.md)

## Reversing
- [assembly](Reversing/assembly.md) — intro to x86-64
- [buffer-overflow](Reversing/buffer-overflow.md) — stack-based overflows (Windows x86)
- [windbg](Reversing/windbg.md) · [game-hacking](Reversing/game-hacking.md)
- [linux-pwn](Reversing/linux-pwn.md) — pwntools, ROP, heap, format strings

## Detection / Blue Team
- [siem](Detection/siem.md) · [linux-forensics](Detection/linux-forensics.md) · [user-behavior-forensics](Detection/user-behavior-forensics.md)
- [malicious-docs](Detection/malicious-docs.md) · [evasion](Detection/evasion.md) · [token-manipulation](Detection/token-manipulation.md)
- [kernel-telemetry](Detection/kernel-telemetry.md) — ETW/WFP
- [privesc-tradecraft](Detection/privesc-tradecraft.md) — UAC bypass, Potato attacks
- [atomic-red-team](Detection/atomic-red-team.md) — + Splunk/Wazuh hunting queries

## CodeReview (Whitebox)
- [whitebox](CodeReview/whitebox.md) · [whitebox-cmdi](CodeReview/whitebox-cmdi.md) · [csharp](CodeReview/csharp.md) · [secure-coding-js](CodeReview/secure-coding-js.md)
- [supply-chain](CodeReview/supply-chain.md) · [git-secrets](CodeReview/git-secrets.md) · [cicd-attacks](CodeReview/cicd-attacks.md)

## Tools
- [shells-payloads](Tools/shells-payloads.md) · [file-transfers](Tools/file-transfers.md) · [metasploit](Tools/metasploit.md) · [sliver](Tools/sliver.md)
- [memory-forensics](Tools/memory-forensics.md) — winpmem/DumpIt acquisition + Volatility analysis (EDR-evasive credential extraction)
- **[tool-index](Tools/tool-index.md) — install command for every tool in this vault**

## General
- [basics](General/basics.md) — terminal basics (tmux/vim)

---

<div align="center">

See **[quality-report.md](quality-report.md)** for source provenance, OCR quality notes, and every correction made along the way.

*For authorized security testing, CTF competitions, and defensive research only.*

</div>
