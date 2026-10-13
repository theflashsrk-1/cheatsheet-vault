#index

# Cheat Sheet Vault — Index

## AD (Active Directory)
- [enumeration](AD/enumeration.md) — unauthenticated/authenticated AD enumeration basics
- [credentialed-enumeration](AD/credentialed-enumeration.md) — Linux-side AD enumeration tooling (BloodHound, ldapsearch, etc.)
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
- [netexec](AD/NetExec.md) — NetExec (nxc) usage
- [exploits](AD/exploits.md) — AD CVE exploits (noPac, PrintNightmare, PetitPotam)
- [adcs](AD/adcs.md) — AD CS abuse, full ESC1–ESC13 chain (Certify/Certipy)

## Windows
- [fundamentals](Windows/fundamentals.md) — Windows fundamentals
- [command-line](Windows/command-line.md) — CMD / PowerShell reference
- [credential-theft](Windows/credential-theft.md) — credential theft (SAM, LSASS, mimikatz)
- [privesc](Windows/privesc.md) — Windows privilege escalation
- [lateral-movement](Windows/lateral-movement.md) — lateral movement techniques
- [attacks-defense](Windows/attacks-defense.md) — Windows attacks & defensive countermeasures
- [process-injection](Windows/process-injection.md) — process injection attacks & detection
- [low-level-detectability](Windows/low-level-detectability.md) — PE format, WinAPI, anti-debug internals
- [persistence](Windows/persistence.md) — persistence mechanisms (Run keys, tasks, services, WMI, COM hijack)

## Linux
- [fundamentals](Linux/fundamentals.md) — Linux fundamentals
- [privesc](Linux/privesc.md) — Linux privilege escalation
- [macos](Linux/macos.md) — macOS fundamentals
- [process-injection-detection](Linux/process-injection-detection.md) — ELF internals, gdb/ptrace, auditctl detection

## Cloud
- [aws](Cloud/aws.md) — IMDS SSRF, IAM privesc, Pacu/CloudFox/ScoutSuite
- [azure](Cloud/azure.md) — Entra ID enum, password spraying, ROADrecon/AzureHound
- [gcp](Cloud/gcp.md) — IMDS SSRF, service-account privesc, ScoutSuite

## Containers
- [docker](Containers/docker.md) — enumeration & container escape (socket, privileged, capabilities)
- [kubernetes](Containers/kubernetes.md) — kubectl enum, RBAC abuse, pod→node escape

## Wireless (Bluetooth / RFID / NFC)
- [bluetooth-rfid](Wireless/bluetooth-rfid.md) — BLE GATT abuse, Proxmark3 (MIFARE/HID)

## Web
- [web-recon](Web/web-recon.md) — web reconnaissance
- [web-requests](Web/web-requests.md) — raw HTTP requests / cURL / APIs
- [web-proxies](Web/web-proxies.md) — Burp Suite / ZAP
- [fuzzing](Web/fuzzing.md) — web fuzzing (multi-tool)
- [ffuf](Web/ffuf.md) — ffuf attack patterns
- [sqli](Web/sqli.md) — SQL injection fundamentals
- [sqli-advanced](Web/sqli-advanced.md) — advanced SQLi (PostgreSQL/Java)
- [blind-sqli](Web/blind-sqli.md) — blind SQL injection
- [sqlmap](Web/sqlmap.md) — sqlmap essentials
- [nosqli](Web/nosqli.md) — NoSQL injection (MongoDB)
- [xss](Web/xss.md) — cross-site scripting
- [csrf-cors-csp](Web/csrf-cors-csp.md) — CSRF / CORS / CSP
- [auth-attacks](Web/auth-attacks.md) — attacking authentication
- [file-inclusion](Web/file-inclusion.md) — LFI/RFI
- [file-upload](Web/file-upload.md) — file upload attacks
- [command-injection](Web/command-injection.md) — OS command injection
- [injection-attacks](Web/injection-attacks.md) — XPath/LDAP/PDF-generator injection
- [deserialization](Web/deserialization.md) — insecure deserialization
- [server-side-attacks](Web/server-side-attacks.md) — SSRF/SSTI/SSI/XSLT
- [http-attacks](Web/http-attacks.md) — CRLF injection / request smuggling
- [http-misconfigurations](Web/http-misconfigurations.md) — HTTP misconfiguration abuse
- [tls-attacks](Web/tls-attacks.md) — HTTPS/TLS attacks
- [graphql](Web/graphql.md) — attacking GraphQL
- [web-attacks](Web/web-attacks.md) — verb tampering / IDOR / XXE
- [common-applications](Web/common-applications.md) — attacking common web apps
- [wordpress](Web/wordpress.md) — hacking WordPress
- [business-logic](Web/business-logic.md) — race conditions, IDOR/BOLA, mass assignment, workflow bypass

## WiFi
- [basics](WiFi/basics.md) — Wi-Fi pentesting basics
- [tools](WiFi/tools.md) — Wi-Fi tools & techniques
- [wep](WiFi/wep.md) — WEP attacks
- [wpa-wpa2](WiFi/wpa-wpa2.md) — attacking WPA/WPA2
- [wpa3](WiFi/wpa3.md) — attacking WPA3
- [wps](WiFi/wps.md) — attacking WPS
- [evil-twin](WiFi/evil-twin.md) — evil twin attacks
- [captive-portals](WiFi/captive-portals.md) — bypassing captive portals
- [password-cracking](WiFi/password-cracking.md) — Wi-Fi password cracking

## Network
- [footprinting](Network/footprinting.md) — service enumeration / footprinting
- [nmap](Network/nmap.md) — nmap network enumeration
- [common-services](Network/common-services.md) — attacking common services (SMB/FTP/SSH/etc.)
- [pivoting](Network/pivoting.md) — pivoting, tunneling & port forwarding
- [traffic-analysis](Network/traffic-analysis.md) — network traffic analysis
- [subdomain-enum](Network/subdomain-enum.md) — subfinder/amass/assetfinder, subdomain takeover

## Passwords
- [password-attacks](Passwords/password-attacks.md) — password attacks overview
- [login-brute-forcing](Passwords/login-brute-forcing.md) — login brute-forcing
- [hashcat](Passwords/hashcat.md) — cracking passwords with hashcat

## Mobile
- [android-fundamentals](Mobile/android-fundamentals.md) — Android fundamentals / ADB
- [static-analysis](Mobile/static-analysis.md) — Android static analysis
- [dynamic-analysis](Mobile/dynamic-analysis.md) — Android dynamic analysis
- [malware-analysis](Mobile/malware-analysis.md) — Android malware analysis
- [automation](Mobile/automation.md) — Android pentest automation
- [ios](Mobile/ios.md) — iOS testing (IPA static analysis, Frida/Objection, Keychain)

## OSINT
- [corporate-recon](OSINT/corporate-recon.md) — corporate OSINT recon

## Reversing
- [assembly](Reversing/assembly.md) — intro to x86-64 assembly
- [buffer-overflow](Reversing/buffer-overflow.md) — stack-based buffer overflows (Windows x86)
- [windbg](Reversing/windbg.md) — dynamic analysis with WinDbg
- [game-hacking](Reversing/game-hacking.md) — game hacking fundamentals
- [linux-pwn](Reversing/linux-pwn.md) — binary exploitation (pwntools, ROP, heap, format strings)

## Detection / Blue Team
- [siem](Detection/siem.md) — SIEM fundamentals (KQL/Elastic)
- [linux-forensics](Detection/linux-forensics.md) — Linux forensics
- [user-behavior-forensics](Detection/user-behavior-forensics.md) — Windows user-behavior DFIR
- [malicious-docs](Detection/malicious-docs.md) — malicious document analysis
- [evasion](Detection/evasion.md) — Windows evasion techniques
- [token-manipulation](Detection/token-manipulation.md) — detecting access-token manipulation
- [kernel-telemetry](Detection/kernel-telemetry.md) — Windows kernel telemetry (ETW/WFP)
- [privesc-tradecraft](Detection/privesc-tradecraft.md) — privesc detection mindset (UAC bypass, Potato attacks)
- [atomic-red-team](Detection/atomic-red-team.md) — Atomic Red Team + Splunk/Wazuh hunting queries

## CodeReview (Whitebox)
- [whitebox](CodeReview/whitebox.md) — whitebox pentesting overview
- [whitebox-cmdi](CodeReview/whitebox-cmdi.md) — whitebox command injection
- [csharp](CodeReview/csharp.md) — C# language reference
- [secure-coding-js](CodeReview/secure-coding-js.md) — secure coding 101: JavaScript
- [supply-chain](CodeReview/supply-chain.md) — supply chain attacks
- [git-secrets](CodeReview/git-secrets.md) — exposed .git dumping, trufflehog/gitleaks, GitHub dorking
- [cicd-attacks](CodeReview/cicd-attacks.md) — GitHub Actions/Jenkins/GitLab CI pipeline abuse

## Tools
- [shells-payloads](Tools/shells-payloads.md) — shells & payloads
- [file-transfers](Tools/file-transfers.md) — file transfer techniques
- [metasploit](Tools/metasploit.md) — Metasploit Framework
- [sliver](Tools/sliver.md) — Sliver C2
- [memory-forensics](Tools/memory-forensics.md) — memory acquisition (winpmem/DumpIt) & Volatility analysis — EDR-evasive credential extraction
- [tool-index](Tools/tool-index.md) — every tool in this vault, one install command each

## General
- [basics](General/basics.md) — terminal basics (tmux/vim)

---
