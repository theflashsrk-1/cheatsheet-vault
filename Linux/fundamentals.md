#linux #fundamentals #bash

# Linux Fundamentals

Core command reference. Related: [privesc](privesc.md) · [macos](macos.md) · [file-transfers](../Tools/file-transfers.md)

## Contents
- [System / Identity](#system--identity)
- [Package Management](#package-management)
- [Files & Navigation](#files--navigation)
- [Text Processing](#text-processing)
- [Permissions](#permissions)
- [Processes & Services](#processes--services)
- [Networking](#networking)

## System / Identity
```bash
id
```
Current user/groups (`whoami`, `hostname`, `uname -a`).

```bash
env
```
Print environment variables.

## Package Management
```bash
apt install <pkg>
```
Debian package mgmt (`dpkg -i`, `snap`, `pip`, `gem`).

## Files & Navigation
```bash
find / -iname "*config*" -type f 2>/dev/null
```
Search the filesystem (`locate` with `updatedb`, `which` for PATH lookup).

```bash
tree <dir>
```
Recursive directory listing.

## Text Processing
```bash
grep <pattern> file
```
Pattern search.

```bash
cat file | cut -d: -f1 | sort | uniq -c
```
Typical filter pipeline (`awk`, `sed`, `tr`, `wc`, `column` for more).

## Permissions
```bash
chmod <mode> <file>
```
Change permissions (`chown user:group <file>` for ownership).

## Processes & Services
```bash
ps aux
```
Process snapshot.

```bash
systemctl status <service>
```
Manage systemd services (`journalctl` for logs, `kill` to signal).

```bash
jobs ; bg ; fg
```
Job control (background/foreground).

## Networking
```bash
ip a
```
Interfaces/addresses (`ifconfig`, `netstat`, `ss`).

```bash
python3 -m http.server
```
Quick HTTP server on port 8000. See [file-transfers](../Tools/file-transfers.md).

```bash
curl -O http://host/file
```
Download a file (`wget` alternative).
