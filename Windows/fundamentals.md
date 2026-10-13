#windows #fundamentals #powershell

# Windows Fundamentals

OS, permissions, services, PowerShell basics. Related: [command-line](command-line.md) · [privesc](privesc.md) · [powershell-management](../AD/powershell-management.md)

## Contents
- [System Info](#system-info)
- [Files & Permissions](#files--permissions)
- [Services & Defender](#services--defender)
- [PowerShell Essentials](#powershell-essentials)

## System Info
```powershell
Get-WmiObject -Class win32_OperatingSystem
```
OS information (`wmic os list brief` in cmd).

```cmd
sconfig
```
Server Core configuration menu.

```cmd
whoami /user
```
View the current user's SID.

## Files & Permissions
```cmd
dir c:\ /a
```
List all files including hidden/system.

```cmd
icacls c:\users /grant joe:f
```
Grant full permissions (`/remove joe` to revoke).

```cmd
tree c:\ /f | more
```
Show directory tree with files, paged.

## Services & Defender
```powershell
Get-Service
```
List services.

```powershell
Get-MpComputerStatus
```
Check Defender protection settings.

## PowerShell Essentials
```powershell
Get-ExecutionPolicy -List
```
View execution policy per scope.

```powershell
Set-ExecutionPolicy Bypass -Scope Process
```
Bypass execution policy for the current session.

```powershell
Get-Module | select Name,ExportedCommands | fl
```
List imported modules and their commands.

```powershell
New-Alias -Name "Show-Files" Get-ChildItem
```
Create an alias (`get-alias` to list).
