# Web

Full web application attack surface — recon, fuzzing, injection, authentication, server-side request handling, and common CMS/app targets. Grouped by sub-area below.

## Recon & Tooling
| Note | Covers |
|---|---|
| [web-recon](web-recon.md) | WHOIS/DNS lookups and general web reconnaissance |
| [web-requests](web-requests.md) | cURL basics and scripting raw HTTP/API requests |
| [web-proxies](web-proxies.md) | Burp Suite / ZAP shortcut reference |

## Fuzzing
| Note | Covers |
|---|---|
| [fuzzing](fuzzing.md) | Multi-tool fuzzing (gobuster, feroxbuster, wordlists) and ffuf filters |
| [ffuf](ffuf.md) | ffuf attack patterns — content discovery, parameter/vhost fuzzing |

## SQL / NoSQL Injection
| Note | Covers |
|---|---|
| [sqli](sqli.md) | SQL injection fundamentals (MySQL-focused) |
| [sqli-advanced](sqli-advanced.md) | Advanced SQLi for PostgreSQL/Java, character/space bypasses |
| [blind-sqli](blind-sqli.md) | Boolean/time/OOB blind SQLi inference, plus MSSQL RCE and file read |
| [sqlmap](sqlmap.md) | sqlmap targeting, tamper scripts, and automation essentials |
| [nosqli](nosqli.md) | NoSQL injection against MongoDB — auth bypass, blind extraction, server-side JS injection |

## Authentication & Client-Side
| Note | Covers |
|---|---|
| [xss](xss.md) | Cross-site scripting detection payloads and exploitation |
| [csrf-cors-csp](csrf-cors-csp.md) | CSRF/CORS/CSP defenses and misconfigurations reference |
| [auth-attacks](auth-attacks.md) | Broken authentication, brute-force bypass, session attacks, JWT/OAuth/SAML |

## Injection & File Attacks
| Note | Covers |
|---|---|
| [file-inclusion](file-inclusion.md) | Local/remote file inclusion (LFI/RFI) and path traversal |
| [file-upload](file-upload.md) | File upload attacks — web shells and filter bypasses |
| [command-injection](command-injection.md) | OS command injection operators and bypass techniques |
| [injection-attacks](injection-attacks.md) | XPath, LDAP, and PDF-generation injection |

## Server-Side & Protocol Attacks
| Note | Covers |
|---|---|
| [deserialization](deserialization.md) | Insecure deserialization — identifying sinks and formats, ysoserial/phpggc |
| [server-side-attacks](server-side-attacks.md) | SSRF, SSTI, SSI, and XSLT injection |
| [http-attacks](http-attacks.md) | CRLF injection and HTTP request smuggling (CL.TE/TE.CL/TE.TE, H2.CL) |
| [http-misconfigurations](http-misconfigurations.md) | Web cache poisoning, host-header attacks, session puzzling |
| [tls-attacks](tls-attacks.md) | HTTPS/TLS attacks and OpenSSL certificate operations |
| [web-attacks](web-attacks.md) | HTTP verb tampering, IDOR, and XXE |

## APIs & Applications
| Note | Covers |
|---|---|
| [graphql](graphql.md) | Attacking GraphQL — data enumeration and introspection |
| [common-applications](common-applications.md) | Attacking Tomcat, Jenkins, Joomla, Drupal, Splunk, and other common apps |
| [wordpress](wordpress.md) | Hacking WordPress — recon and WPScan |

## Business Logic
| Note | Covers |
|---|---|
| [business-logic](business-logic.md) | Race conditions (single-packet attack), IDOR/BOLA, mass assignment, workflow bypass |
