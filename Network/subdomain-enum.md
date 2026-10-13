#network #osint #recon #dns

# Subdomain & DNS Enumeration

Related: [web-recon](../Web/web-recon.md) · [footprinting](footprinting.md) · [corporate-recon](../OSINT/corporate-recon.md)

## Passive Subdomain Discovery
```bash
subfinder -d target.com -o subs.txt
subfinder -d target.com -all -recursive -o subs.txt
```
Aggregates passive sources (cert transparency, search engines, APIs). `-all` enables every configured source; `-recursive` re-runs on discovered subdomains that support further enumeration.

```bash
assetfinder target.com --subs-only
```
Fast passive lookup via certificate transparency + a handful of APIs; `--subs-only` drops unrelated root-domain results.

```bash
amass enum -passive -d target.com -o amass_passive.txt
amass intel -org "Target Org Name"
```
`enum -passive` uses only passive sources (no direct DNS to the target); `intel -org` resolves an organization name to its ASNs/netblocks as a separate recon step.

```bash
amass enum -active -d target.com -brute -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```
Active mode adds direct DNS brute-forcing and zone-walking on top of passive sources — louder, more complete.

## Resolving & Probing Live Hosts
```bash
cat subs.txt | httpx -title -sc -td
httpx -l subs.txt -mc 200,301,302,403 -o live.txt
```
`httpx` resolves each candidate and reports the HTTP status, title, and tech-detect for anything alive — turns a raw subdomain list into a prioritized target list.

```bash
cat subs.txt | dnsx -resp
```
Bulk-resolve a subdomain list to IPs, filtering out anything that doesn't resolve at all.

## Certificate Transparency (Manual)
```bash
curl -s "https://crt.sh/?q=%.target.com&output=json" | jq -r '.[].name_value' | sort -u
```
Direct crt.sh query — useful when you want raw CT-log data without a wrapper tool, or to cross-check `subfinder`'s cert-transparency source.

## Zone Transfer (Misconfigured DNS)
```bash
dig axfr @ns1.target.com target.com
fierce --domain target.com
```
If a nameserver allows unauthenticated zone transfers (rare but still found), this dumps the entire DNS zone in one query — complete subdomain list with no guessing.

## Subdomain Takeover Check
```bash
subjack -w subs.txt -t 50 -timeout 30 -o takeovers.txt -ssl
```
Flags subdomains with a CNAME pointing to a decommissioned cloud/SaaS resource (S3, Heroku, Azure, GitHub Pages, etc.) that can be claimed to serve attacker content from the victim's domain.

See [web-recon](../Web/web-recon.md) for virtual-host discovery and directory/content discovery once hosts are identified.
