#osint #recon #passive

# OSINT: Corporate Recon

Mostly a resource/tool index (passive recon). Related: [web-recon](../Web/web-recon.md) · [footprinting](../Network/footprinting.md)

## Contents
- [Methodology Checklist](#methodology-checklist)
- [Tools (runnable)](#tools-runnable)
- [Domain / Infrastructure](#domain--infrastructure)
- [People / Email](#people--email)
- [Leaks / Archives](#leaks--archives)
- [Code / Social](#code--social)

## Methodology Checklist
- [ ] Enumerate subdomains from Certificate Transparency logs
- [ ] Harvest emails/subdomains/hosts from public sources (theHarvester)
- [ ] Check target emails against known breach/credential dumps
- [ ] Pull metadata (authors, software, paths) from public documents
- [ ] Map domain/DNS/tech-stack footprint and look for exposed cloud storage
- [ ] Identify staff, emails, and company org structure
- [ ] Search leak/archive/vuln sources for breach data and historical snapshots
- [ ] Search code-hosting and social platforms for leaked secrets and footprint

## Tools (runnable)
```bash
python3 CTFR.py -d example.com
```
Enumerate subdomains from Certificate Transparency.

```bash
theHarvester -d example.com -b all
```
Gather emails/subdomains/hosts from public sources.

```bash
h8mail -t target@example.com
```
Hunt breached credentials for an email.

```bash
exiftool document.pdf
```
Extract metadata (authors, software, paths) from files.

## Domain / Infrastructure
| Resource | Purpose |
|---|---|
| `crt.sh` | Certificate Transparency log search (subdomain discovery) |
| `dnsdumpster.com` | Passive DNS recon / subdomain mapping |
| `mxtoolbox.com` | MX/DNS/blacklist lookup toolkit |
| `ICANN Lookup` | WHOIS registrant lookup |
| `ARIN` | IP allocation / ASN registry (Americas) |
| `securitytrails.com` | Historical DNS & WHOIS records |
| `buckets.grayhatwarfare.com` | Search engine for open/misconfigured cloud storage buckets |
| `builtwith.com` | Website technology-stack fingerprinting |
| `shodan.io` | Internet-connected device/service search engine |
| Cloud IP ranges: AWS / Azure / GCP / DigitalOcean | Published provider IP ranges, for scoping cloud-hosted assets |

Map domains, DNS, tech stack, and exposed cloud storage.

## People / Email
| Resource | Purpose |
|---|---|
| `hunter.io` | Corporate email address discovery / pattern finder |
| `emailrep.io` | Email reputation/risk lookup |
| `rocketreach.co` | Contact/email lookup by name + company |
| `allpeople.com` | Company org-chart / employee directory |
| `crunchbase.com` | Company profile, funding, and personnel data |
| `tineye.com` + `images.google.com` | Reverse image search for photo attribution |

Identify staff, emails, and company structure.

## Leaks / Archives
| Resource | Purpose |
|---|---|
| `haveibeenpwned.com` | Breach exposure lookup by email/domain |
| Rapid7 FDNS | Forward DNS dataset for passive recon |
| `pastebin.com` | Public paste search for leaked data |
| `virustotal.com` | File/URL/domain reputation and relationship graph |
| `archive.org` + `archive.fo` | Wayback / historical page snapshots |
| `exploit-db.com` | Public exploit database |
| `cvedetails.com` | CVE/vulnerability lookup by product/vendor |

Breach data, historical snapshots, and known-vuln lookup.

## Code / Social
| Resource | Purpose |
|---|---|
| `github.com` / `gitlab.com` / `bitbucket.org` | Source-code hosting (secrets/config leaks) |
| `searchcode.com` | Source-code search engine across public repos |
| LinkedIn / Twitter(X) / Facebook / Instagram / Reddit / StackOverflow | Social footprint and technical Q&A exposure |

Source-code leaks and social footprint.
