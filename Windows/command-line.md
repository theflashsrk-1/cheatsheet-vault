#windows #cmd #powershell #cli

# Windows Command Line (CMD / PowerShell)

Built-in administration reference. Related: [fundamentals](fundamentals.md) · [privesc](privesc.md) · [credential-theft](credential-theft.md)

## Contents
- [General](#general)
- [Files & Directories](#files--directories)
- [Users & Groups](#users--groups)
- [Active Directory (RSAT)](#active-directory-rsat)
- [Networking](#networking)
- [Services](#services)
- [Scheduled Tasks](#scheduled-tasks)
- [Remote Execution](#remote-execution)
- [Downloads & Events](#downloads--events)

## General
```cmd
systeminfo
```
OS configuration info.

```powershell
Get-Help <cmdlet>
```
Help for a cmdlet (`help <command>` in cmd; `Update-Help` to refresh).

```cmd
doskey /history
```
Print the session's command history.

## Files & Directories
```cmd
dir /A:H
```
List hidden files (`/A:R` read-only).

```cmd
robocopy /E /MIR /A-:SH [source] [destination]
```
Mirror a directory, clearing system/hidden attributes.

```cmd
fsutil file createNew <filename> <length>
```
Create a file of a given size.

```cmd
echo "string" >> <filename>
```
Append to a file (`>` to overwrite).

```powershell
Get-ChildItem -Path C:\Users\MTanaka\ -Filter "*.txt" -Recurse
```
Recursively find files by pattern.

```powershell
Get-Item <item> | Select-Object -Property *
```
View all properties of an object (pipe to `Get-Member` to see members).

## Users & Groups
```cmd
net user
```
List local users (`net group` / `net localgroup` for groups).

```powershell
New-LocalUser -Name "username" -NoPassword
```
Create a local user (`Set-LocalUser` to modify).

```powershell
Get-LocalGroupMember -Name "Administrators"
```
List members of a local group.

## Active Directory (RSAT)
```powershell
Get-ADUser -Filter {EmailAddress -like '*greenhorn.corp'}
```
Query AD users by attribute. See [enumeration](../AD/enumeration.md).

```powershell
New-ADUser -Name "UserName" -Surname "Last" -GivenName "First"
```
Create an AD user. See [powershell-management](../AD/powershell-management.md).

## Networking
```cmd
ipconfig /all
```
Full network configuration.

```cmd
netstat -an
```
Active connections and listening ports.

```powershell
Get-NetIPAddress
```
IP configuration (`Get-NetRoute`, `Get-NetNeighbor` for routes/ARP).

```powershell
Test-NetConnection -ComputerName 10.129.224.248 -Port 445
```
Test connectivity to a host/port.

## Services
```cmd
sc query <Name>
```
Query a service (`sc start/stop/config`).

```powershell
Get-Service | where DisplayName -like '*Defender*'
```
Filter services.

```powershell
Set-Service -Name <Name> -StartType Disabled
```
Change service start type.

## Scheduled Tasks
```cmd
schtasks /query /V /FO list
```
List all scheduled tasks in detail.

```cmd
schtasks /create /sc <schedule> /tn <TaskName> /tr <command>
```
Create a scheduled task (`/change`, `/delete` to modify/remove).

## Remote Execution
```powershell
Invoke-Command -ComputerName ACADEMY-ICL-DC -ScriptBlock { Get-Service }
```
Run a command on a remote host. See [lateral-movement](lateral-movement.md).

## Downloads & Events
```powershell
Invoke-WebRequest -Uri "https://site/file" -OutFile file
```
Download a file. See [file-transfers](../Tools/file-transfers.md).

```powershell
Get-WinEvent -LogName 'Security' -MaxEvents 5
```
Read event logs (`-FilterHashTable` to filter).
