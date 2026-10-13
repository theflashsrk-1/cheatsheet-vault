#codereview #javascript #deobfuscation #secure-coding

# Secure Coding 101: JavaScript

Deobfuscation + review tooling. Related: [whitebox](whitebox.md) · [xss](../Web/xss.md) · [nosqli](../Web/nosqli.md) · [web-requests](../Web/web-requests.md)

## Requests (testing deobfuscated endpoints)
```bash
curl http://SERVER_IP:PORT/
```
cURL GET request.

```bash
curl -s http://SERVER_IP:PORT/ -X POST -d "param1=sample"
```
cURL POST request with form data.

## Encoding / Decoding
```bash
echo hackthebox | base64
echo ENCODED_B64 | base64 -d
```
base64 encode / decode.

```bash
echo hackthebox | xxd -p
echo ENCODED_HEX | xxd -p -r
```
Hex encode / decode.

```bash
echo hackthebox | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
ROT13 encode (same command decodes — the cipher is symmetric).

## VSCode Shortcuts
```
ctrl+a ctrl+k ctrl+0   Fold all functions
alt+shift+F12          Find all references
ctrl+/                 Comment out
ctrl+`                 Open terminal
```

## Firefox DevTools
```
ctrl+u          View source
ctrl+shift+k    Console
ctrl+shift+c    Debugger/inspector
F11             Step in
```

## Deobfuscation Resources
```
Prettier / Beautifier / JSNice / JS Console
```
Reformat and rename obfuscated JavaScript to aid review.

## Review Focus
```
- eval / Function() / setTimeout(string)  (see [whitebox](whitebox.md))
- Prototype pollution via __proto__
- Unsanitized DOM sinks -> XSS (see [xss](../Web/xss.md))
```
Common JS weaknesses to flag.
