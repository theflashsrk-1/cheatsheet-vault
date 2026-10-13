#web #recon #osint #dns #subdomains

# Web Reconnaissance

Related: [fuzzing](fuzzing.md) · [footprinting](../Network/footprinting.md) · [corporate-recon](../OSINT/corporate-recon.md)

## WHOIS / DNS
```bash
whois example.com
```
Registrar, dates, nameservers, contacts.

```bash
dig example.com A
```
Query a DNS record (A/AAAA/CNAME/MX/NS/TXT/SOA).

```bash
dig @ns1.example.com example.com axfr
```
Attempt a DNS zone transfer (AXFR) — dumps the whole zone if misconfigured.

## Subdomain Enumeration
```bash
dnsenum example.com -f subdomains.txt
```
Brute-force subdomains with a wordlist.

```bash
curl -s "https://crt.sh/?q=%25.example.com&output=json" | jq -r '.[].name_value' | sed 's/\*\.//g' | sort -u
```
Passive subdomain discovery via Certificate Transparency logs.

## Virtual Hosts
```bash
gobuster vhost -u http://192.0.2.1 -w hostnames.txt
```
Enumerate virtual hosts sharing an IP. See [ffuf](ffuf.md).

## Crawling
```bash
# robots.txt first
curl -s http://example.com/robots.txt
```
Reveals disallowed/hidden paths.

```bash
jq -r '.[] | select(.file != null) | .file' example_data.json | sort -u
```
Extract links from Scrapy crawl output.

## Search Engine (Google Dorks)
```
site:example.com "password reset"
inurl:admin login
filetype:pdf "confidential report"
intitle:"index of" /backup
```
OSINT via advanced search operators. See [corporate-recon](../OSINT/corporate-recon.md).

## Web Archives
```
https://web.archive.org/web/*/example.com
```
Wayback Machine — historical snapshots, removed/hidden content.
