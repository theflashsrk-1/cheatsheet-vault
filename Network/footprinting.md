#network #footprinting #enumeration #snmp #nfs

# Footprinting (Service Enumeration)

Per-service enumeration. Related: [nmap](nmap.md) · [common-services](common-services.md) · [web-recon](../Web/web-recon.md) · [corporate-recon](../OSINT/corporate-recon.md)

## Contents
- [Infrastructure](#infrastructure)
- [FTP](#ftp)
- [SMB](#smb)
- [NFS](#nfs)
- [DNS](#dns)
- [SMTP / IMAP / POP3](#smtp--imap--pop3)
- [SNMP](#snmp)
- [Databases](#databases)
- [Remote Management](#remote-management)
- [Oracle TNS](#oracle-tns)
- [IPMI](#ipmi)

## Recon Checklist
- [ ] Infrastructure: certificate transparency (crt.sh) + Shodan lookups
- [ ] FTP: anonymous access, encrypted (TLS) connection
- [ ] SMB: null-session shares, RPC, SAMR users, automated enum (`enum4linux-ng`)
- [ ] NFS: list + mount exports
- [ ] DNS: zone transfer attempt, subdomain brute-force
- [ ] SMTP/IMAP/POP3: user enum (VRFY/EXPN/RCPT), protocol login
- [ ] SNMP: community-string brute-force, OID walk
- [ ] Databases: MySQL / MSSQL login attempts
- [ ] Remote Management: SSH config audit, WinRM, WMI
- [ ] Oracle TNS: component enum, DB login
- [ ] IPMI: hash dump (Metasploit)

## Infrastructure
```bash
curl -s "https://crt.sh/?q=<target-domain>&output=json" | jq .
```
Certificate Transparency subdomain discovery.

```bash
for i in $(cat ip-addresses.txt); do shodan host $i; done
```
Look up each IP in Shodan.

## FTP
```bash
wget -m --no-passive ftp://anonymous:anonymous@<target>
```
Mirror all files from an anonymous FTP server.

```bash
openssl s_client -connect <FQDN/IP>:21 -starttls ftp
```
Interact over an encrypted FTP connection.

## SMB
```bash
smbclient -N -L //<FQDN/IP>
```
Null-session share listing.

```bash
rpcclient -U "" <FQDN/IP>
```
RPC interaction (null session).

```bash
samrdump.py <FQDN/IP>
```
Enumerate users via Impacket SAMR.

```bash
enum4linux-ng.py <FQDN/IP> -A
```
Automated SMB enumeration. See [credentialed-enumeration](../AD/credentialed-enumeration.md).

## NFS
```bash
showmount -e <FQDN/IP>
```
List exported NFS shares.

```bash
mount -t nfs <FQDN/IP>:/<share> ./target-NFS/ -o nolock
```
Mount an NFS share.

## DNS
```bash
dig axfr <domain.tld> @<nameserver>
```
Zone transfer (also `dig ns`, `dig any`).

```bash
dnsenum --dnsserver <nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list <domain.tld>
```
Subdomain brute-force.

## SMTP / IMAP / POP3
```bash
telnet <FQDN/IP> 25
```
Interact with SMTP (VRFY/EXPN/RCPT).

```bash
openssl s_client -connect <FQDN/IP>:imaps
```
Connect to IMAPS (also `pop3s`); `curl -k 'imaps://<FQDN/IP>' --user u:p` to log in.

## SNMP
```bash
snmpwalk -v2c -c <community string> <FQDN/IP>
```
Walk OIDs.

```bash
onesixtyone -c community-strings.list <FQDN/IP>
```
Brute-force SNMP community strings (also `braa <community>@<IP>:.1.*`).

## Databases
```bash
mysql -u <user> -p<password> -h <FQDN/IP>
```
MySQL login.

```bash
mssqlclient.py <user>@<FQDN/IP> -windows-auth
```
MSSQL login (Windows auth). See [common-services](common-services.md).

## Remote Management
```bash
ssh-audit.py <FQDN/IP>
```
Audit SSH configuration.

```bash
evil-winrm -i <FQDN/IP> -u <user> -p <password>
```
Log in over WinRM. See [lateral-movement](../Windows/lateral-movement.md).

```bash
wmiexec.py <user>:"<password>"@<FQDN/IP> "<command>"
```
Execute via WMI.

## Oracle TNS
```bash
./odat.py all -s <FQDN/IP>
```
Enumerate Oracle DB components.

```bash
sqlplus <user>/<pass>@<FQDN/IP>/<db>
```
Log in to an Oracle database.

## IPMI
```text
msf6 auxiliary(scanner/ipmi/ipmi_dumphashes)
```
Dump IPMI hashes (Metasploit). See [hashcat](../Passwords/hashcat.md).
