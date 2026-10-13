#tools #install #reference

# Tool Index — Install Reference

Every tool referenced anywhere in this vault, with how to actually get it on a box. Package manager first; `go install`/`pip`/`pipx`/`gem` next; a bare GitHub Releases download only when the project ships no package at all (compiled .NET tools, Windows-only binaries). Skip straight to the section you need — each links back to where it's actually used.

## Contents
- [AD / Windows](#ad--windows)
- [Passwords](#passwords)
- [Web](#web)
- [Network / Recon](#network--recon)
- [Tools (Shells / C2)](#tools-shells--c2)
- [Mobile](#mobile)
- [Reversing](#reversing)
- [Detection / Forensics](#detection--forensics)
- [Cloud](#cloud)
- [Containers](#containers)
- [Wireless](#wireless)
- [Secrets / CI-CD / OSINT](#secrets--ci-cd--osint)

## AD / Windows
| Tool | Install | Used in |
|---|---|---|
| Impacket | `apt install python3-impacket` (Kali) · `pipx install impacket` | [kerberoasting](../AD/kerberoasting.md), [dcsync](../AD/dcsync.md), [ntlm-relay](../AD/ntlm-relay.md) |
| NetExec (`nxc`) | `apt install netexec` (Kali) · `pipx install git+https://github.com/Pennyw0rth/NetExec` | [netexec](../AD/NetExec.md) |
| BloodHound CE (GUI) | official `docker-compose` deploy — see [bloodhound.specterops.io](https://bloodhound.specterops.io) | [credentialed-enumeration](../AD/credentialed-enumeration.md) |
| bloodhound-python (ingestor) | `pipx install bloodhound` | [enumeration](../AD/enumeration.md) |
| Responder | `apt install responder` (Kali) | [password-spraying](../AD/password-spraying.md) |
| evil-winrm | `gem install evil-winrm` | [lateral-movement](../Windows/lateral-movement.md) |
| Coercer | `pipx install coercer` | [ntlm-relay](../AD/ntlm-relay.md) |
| ldapdomaindump | `pipx install ldapdomaindump` | [enumeration](../AD/enumeration.md) |
| enum4linux-ng | `pipx install enum4linux-ng` | [credentialed-enumeration](../AD/credentialed-enumeration.md) |
| kerbrute | `go install github.com/ropnop/kerbrute@latest` | [password-spraying](../AD/password-spraying.md) |
| Certipy | `pipx install certipy-ad` | [adcs](../AD/adcs.md) |
| Rubeus | prebuilt release (GhostPack/Rubeus, GitHub Releases) — compiled .NET assembly, no package manager; load reflectively with `Invoke-ReflectivePEInjection`/execute-assembly to avoid dropping the binary to disk | [kerberos-tickets](../AD/kerberos-tickets.md), [attacks-defense](../Windows/attacks-defense.md) |
| Certify | prebuilt release (GhostPack/Certify, GitHub Releases) — same story as Rubeus | [adcs](../AD/adcs.md) |
| Mimikatz | prebuilt release (gentilkiwi/mimikatz, GitHub Releases) — near-universally signature-flagged by AV/EDR; prefer running it in-memory via PowerSploit's `Invoke-Mimikatz.ps1` cradle or a C2's built-in module over dropping the .exe | [credential-theft](../Windows/credential-theft.md), [kernel-telemetry](../Detection/kernel-telemetry.md) |
| PowerView / PowerSploit | `git clone https://github.com/PowerShellMafia/PowerSploit` then `Import-Module .\PowerSploit.psd1`, or load in-memory with an `IEX (New-Object Net.WebClient).DownloadString(...)` cradle | [powershell-management](../AD/powershell-management.md), [attacks-defense](../Windows/attacks-defense.md) |
| noPac / PetitPotam / PrintNightmare PoCs | `git clone` the specific PoC repo (e.g. `Ridter/noPac`, `topotam/PetitPotam`) — one-off Python scripts, no package to install | [exploits](../AD/exploits.md) |

## Passwords
| Tool | Install | Used in |
|---|---|---|
| hashcat | `apt install hashcat` | [hashcat](../Passwords/hashcat.md) note in Passwords |
| John the Ripper | `apt install john` | [password-attacks](../Passwords/password-attacks.md) |
| Hydra | `apt install hydra` | [login-brute-forcing](../Passwords/login-brute-forcing.md) |

## Web
| Tool | Install | Used in |
|---|---|---|
| ffuf | `apt install ffuf` · `go install github.com/ffuf/ffuf/v2@latest` | [ffuf](../Web/ffuf.md) |
| gobuster | `apt install gobuster` · `go install github.com/OJ/gobuster/v3@latest` | [fuzzing](../Web/fuzzing.md) |
| sqlmap | `apt install sqlmap` | [sqlmap](../Web/sqlmap.md) |
| Burp Suite (Community) | manual install — no clean package-manager path, grab the installer from portswigger.net; Kali ships `burpsuite` via apt but it's frequently out of date | [web-proxies](../Web/web-proxies.md) |
| OWASP ZAP | `apt install zaproxy` | [web-proxies](../Web/web-proxies.md) |
| WPScan | `gem install wpscan` (Kali ships `apt install wpscan`) | [wordpress](../Web/wordpress.md) |
| Nikto | `apt install nikto` | [web-recon](../Web/web-recon.md) |
| WhatWeb | `apt install whatweb` | [web-recon](../Web/web-recon.md) |

## Network / Recon
| Tool | Install | Used in |
|---|---|---|
| nmap | `apt install nmap` | [nmap](../Network/nmap.md) |
| masscan | `apt install masscan` | [footprinting](../Network/footprinting.md) |
| Wireshark / tshark | `apt install wireshark` | [traffic-analysis](../Network/traffic-analysis.md) |
| proxychains4 | `apt install proxychains4` | [pivoting](../Network/pivoting.md) |
| chisel | `go install github.com/jpillora/chisel@latest` | [pivoting](../Network/pivoting.md) |
| ligolo-ng | prebuilt release (nicocha30/ligolo-ng, GitHub Releases) — ships as a proxy binary + agent, no package | [pivoting](../Network/pivoting.md) |
| subfinder | `go install github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest` | [subdomain-enum](../Network/subdomain-enum.md) |
| amass | `apt install amass` · `go install github.com/owasp-amass/amass/v4/...@master` | [subdomain-enum](../Network/subdomain-enum.md) |
| assetfinder | `go install github.com/tomnomnom/assetfinder@latest` | [subdomain-enum](../Network/subdomain-enum.md) |
| httpx / dnsx | `go install github.com/projectdiscovery/httpx/cmd/httpx@latest` / `.../dnsx/cmd/dnsx@latest` | [subdomain-enum](../Network/subdomain-enum.md) |
| subjack | `go install github.com/haccer/subjack@latest` | [subdomain-enum](../Network/subdomain-enum.md) |

## Tools (Shells / C2)
| Tool | Install | Used in |
|---|---|---|
| Metasploit Framework | `apt install metasploit-framework` (Kali) · official omnibus installer (`curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb \| sudo bash`) elsewhere | [metasploit](metasploit.md) |
| Sliver | `curl https://sliver.sh/install \| sudo bash` (official installer script) | [sliver](sliver.md) |
| ncat / netcat | `apt install nmap` (bundles `ncat`) · `apt install netcat-traditional` | [shells-payloads](shells-payloads.md) |

## Mobile
| Tool | Install | Used in |
|---|---|---|
| adb | `apt install android-tools-adb` | [android-fundamentals](../Mobile/android-fundamentals.md) |
| apktool | `apt install apktool` | [static-analysis](../Mobile/static-analysis.md) |
| jadx | `apt install jadx` (or prebuilt release for the GUI) | [static-analysis](../Mobile/static-analysis.md) |
| frida-tools | `pip install frida-tools` | [dynamic-analysis](../Mobile/dynamic-analysis.md), [ios](../Mobile/ios.md) |
| objection | `pip install objection` | [dynamic-analysis](../Mobile/dynamic-analysis.md), [ios](../Mobile/ios.md) |
| MobSF | `docker run -it --rm -p 8000:8000 opensecurity/mobile-security-framework-mobsf:latest` | [automation](../Mobile/automation.md) |
| class-dump | `brew install class-dump` (macOS; needed for iOS Obj-C header recovery) | [ios](../Mobile/ios.md) |

## Reversing
| Tool | Install | Used in |
|---|---|---|
| gdb | `apt install gdb` | [linux-pwn](../Reversing/linux-pwn.md) |
| GEF | `bash -c "$(curl -fsSL https://gef.blah.cat/sh)"` (official installer, wires into `~/.gdbinit`) | [linux-pwn](../Reversing/linux-pwn.md) |
| pwntools | `pip install pwntools` | [linux-pwn](../Reversing/linux-pwn.md) |
| ROPgadget | `pip install ropgadget` | [linux-pwn](../Reversing/linux-pwn.md) |
| one_gadget | `gem install one_gadget` | [linux-pwn](../Reversing/linux-pwn.md) |
| radare2 | `apt install radare2` | [assembly](../Reversing/assembly.md) |
| Ghidra | prebuilt release (NationalSecurityAgency/ghidra, GitHub Releases) — Java-based, no package manager install | [windbg](../Reversing/windbg.md) |
| x64dbg | prebuilt release (x64dbg/x64dbg, GitHub Releases) — Windows GUI only | [process-injection](../Windows/process-injection.md) |

## Detection / Forensics
| Tool | Install | Used in |
|---|---|---|
| Volatility 3 | `pip install volatility3` | [memory-forensics](memory-forensics.md) |
| winpmem | prebuilt release (Velocidex/WinPmem, GitHub Releases) — direct unauthenticated download | [memory-forensics](memory-forensics.md) |
| Magnet DumpIt for Windows | registration-gated download — magnetforensics.com/resources/magnet-dumpit-for-windows/ | [memory-forensics](memory-forensics.md) |
| YARA | `apt install yara` | [malicious-docs](../Detection/malicious-docs.md) |
| Sysinternals Suite | manual download, no apt package (Windows) — learn.microsoft.com/sysinternals | [kernel-telemetry](../Detection/kernel-telemetry.md) |

## Cloud
| Tool | Install | Used in |
|---|---|---|
| AWS CLI | `apt install awscli` · `pip install awscli` | [aws](../Cloud/aws.md) |
| Azure CLI | `curl -sL https://aka.ms/InstallAzureCLIDeb \| sudo bash` (official) | [azure](../Cloud/azure.md) |
| gcloud CLI | Google Cloud SDK apt repo, or `curl https://sdk.cloud.google.com \| bash` (official) | [gcp](../Cloud/gcp.md) |
| Pacu | `pip install pacu` | [aws](../Cloud/aws.md) |
| CloudFox | `go install github.com/BishopFox/cloudfox@latest` | [aws](../Cloud/aws.md) |
| ScoutSuite | `pip install scoutsuite` | [aws](../Cloud/aws.md), [gcp](../Cloud/gcp.md) |
| ROADrecon | `pip install roadrecon` | [azure](../Cloud/azure.md) |
| AzureHound | prebuilt release (BloodHoundAD/AzureHound, GitHub Releases) · `go install github.com/bloodhoundad/azurehound/v2@latest` | [azure](../Cloud/azure.md) |
| MicroBurst | `git clone https://github.com/NetSPI/MicroBurst` then `Import-Module .\MicroBurst.psm1` — PowerShell module, no package manager | [azure](../Cloud/azure.md) |

## Containers
| Tool | Install | Used in |
|---|---|---|
| kubectl | `apt install kubectl` (via Google's apt repo) · `snap install kubectl --classic` | [kubernetes](../Containers/kubernetes.md) |
| kube-hunter | `pip install kube-hunter` | [kubernetes](../Containers/kubernetes.md) |
| kubectl-who-can | `go install github.com/aquasecurity/kubectl-who-can@latest` | [kubernetes](../Containers/kubernetes.md) |

## Wireless
| Tool | Install | Used in |
|---|---|---|
| aircrack-ng suite | `apt install aircrack-ng` | [wpa-wpa2](../WiFi/wpa-wpa2.md) |
| hcxtools | `apt install hcxtools` | [hashcat](../Passwords/hashcat.md) |
| bettercap | `apt install bettercap` | [bluetooth-rfid](../Wireless/bluetooth-rfid.md) |
| Proxmark3 client | `apt install proxmark3` (Kali) — or build the RRG/Iceman fork from source for current firmware support | [bluetooth-rfid](../Wireless/bluetooth-rfid.md) |
| hostapd-wpe | `apt install hostapd-wpe` | [evil-twin](../WiFi/evil-twin.md) |
| reaver | `apt install reaver` | [wps](../WiFi/wps.md) |

## Secrets / CI-CD / OSINT
| Tool | Install | Used in |
|---|---|---|
| trufflehog | `go install github.com/trufflesecurity/trufflehog/v3@latest` · `brew install trufflehog` | [git-secrets](../CodeReview/git-secrets.md) |
| gitleaks | `apt install gitleaks` · `brew install gitleaks` · `go install github.com/gitleaks/gitleaks/v8@latest` | [git-secrets](../CodeReview/git-secrets.md) |
| git-dumper | `pipx install git-dumper` | [git-secrets](../CodeReview/git-secrets.md) |
| theHarvester | `apt install theharvester` | [corporate-recon](../OSINT/corporate-recon.md) |
| Sherlock | `pipx install sherlock-project` | [corporate-recon](../OSINT/corporate-recon.md) |
| h8mail | `pip install h8mail` | [corporate-recon](../OSINT/corporate-recon.md) |
| Recon-ng | `apt install recon-ng` | [corporate-recon](../OSINT/corporate-recon.md) |
