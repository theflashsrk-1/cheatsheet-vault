#web #fuzzing #gobuster #feroxbuster #wordlists

# Web Fuzzing (multi-tool)

Related: [ffuf](ffuf.md) · [file-inclusion](file-inclusion.md) · [login-brute-forcing](../Passwords/login-brute-forcing.md)

## ffuf Filters
```bash
ffuf -u http://example.com/FUZZ -w wordlist.txt -mc 200
```
Match only specific status codes.

```bash
ffuf -u http://example.com/FUZZ -w wordlist.txt -mr "Welcome"
```
Match responses by regex.

```bash
ffuf -u http://example.com/FUZZ -w wordlist.txt -e .php,.html
```
Append extensions to each entry.

```bash
ffuf -u http://example.com/FUZZ -w wordlist.txt -t 50 -x http://127.0.0.1:8080
```
Set threads and route through a proxy. See [web-proxies](web-proxies.md).

## gobuster
```bash
gobuster dir -u http://example.com -w wordlist.txt -x .php,.html
```
Directory fuzzing with extensions.

```bash
gobuster dir -u http://example.com -w wordlist.txt -s 200 -b ''
```
Filter by status code.

```bash
gobuster dns -d example.com -w subdomains.txt -i
```
DNS subdomain fuzzing, showing IPs.

## feroxbuster (recursive)
```bash
feroxbuster -u http://example.com -w wordlist.txt -x php,html
```
Recursive discovery with extensions.

```bash
feroxbuster -u http://example.com -w wordlist.txt -C 404 --depth 3
```
Exclude 404s, cap recursion depth.

```bash
feroxbuster -u http://example.com -w wordlist.txt --no-recursion
```
Disable recursion.

## wenum (wfuzz fork)
```bash
wenum -c -w wordlist.txt --hc 404 -u http://example.com/FUZZ
```
Fuzz, hide 404 responses.

```bash
wenum -c -w wordlist.txt -d 'username=FUZZ&password=secret' -u http://example.com/login
```
Fuzz POST form data. See [login-brute-forcing](../Passwords/login-brute-forcing.md).

## Common SecLists Wordlists
```
/usr/share/seclists/Discovery/Web-Content/common.txt
/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
/usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt
/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```
Go-to content/DNS wordlists.
