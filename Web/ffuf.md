#web #ffuf #fuzzing #enumeration

# ffuf (Attack Patterns)

Related: [fuzzing](fuzzing.md) · [file-inclusion](file-inclusion.md) · [web-requests](web-requests.md) · [footprinting](../Network/footprinting.md)

## Content Discovery
```bash
ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ
```
Directory/file fuzzing.

```bash
ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/indexFUZZ
```
Extension fuzzing.

```bash
ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php
```
Page fuzzing within a directory.

```bash
ffuf -w wordlist.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v
```
Recursive fuzzing with an extension.

## DNS / VHost
```bash
ffuf -w wordlist.txt:FUZZ -u https://FUZZ.hackthebox.eu/
```
Sub-domain fuzzing (DNS-resolved).

```bash
ffuf -w wordlist.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb' -fs xxx
```
VHost fuzzing (filter the default-response size with `-fs`).

## Parameters & Values
```bash
ffuf -w wordlist.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=key -fs xxx
```
GET parameter name fuzzing.

```bash
ffuf -w wordlist.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```
POST parameter name fuzzing.

```bash
ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```
Parameter value fuzzing.

## Helpers
```bash
sudo sh -c 'echo "SERVER_IP academy.htb" >> /etc/hosts'
```
Add a local DNS entry (needed for vhost testing).

```bash
for i in $(seq 1 1000); do echo $i >> ids.txt; done
```
Generate a numeric sequence wordlist.
