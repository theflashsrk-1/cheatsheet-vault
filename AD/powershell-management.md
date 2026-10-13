#ad #powershell #management

# AD PowerShell Management

Admin cmdlets for creating/modifying AD objects (useful post-compromise for persistence). Related: [enumeration](enumeration.md) · [fundamentals](../Windows/fundamentals.md)

## Contents
- [Connectivity](#connectivity)
- [Discovery](#discovery)
- [Users](#users)
- [Groups / OUs](#groups--ous)
- [GPOs](#gpos)
- [Computers](#computers)

> [!NOTE]
> These cmdlets require AD admin rights (Domain Admin or delegated OU control) and modify real directory objects — creating users/groups/GPOs or resetting passwords are all persistence-grade, auditable actions (new-object and password-reset events are commonly alerted on).

## Connectivity
```powershell
xfreerdp /v:<IP> /u:<User> /p:<Password>
```
RDP to a target.

## Discovery
```powershell
Get-Module
```
List loaded PowerShell modules.

```powershell
Get-Command -Module ActiveDirectory
```
List commands provided by a module.

```powershell
Get-Help <cmd-let>
```
Show help/syntax for a cmdlet.

## Users
```powershell
New-ADUser -Name "first last" -AccountPassword (Read-Host -AsSecureString "Super$ecurePassword!") -Enabled $true -OtherAttributes @{'title'="Analyst";'mail'="f.last@domain.com"}
```
Create a user and set attributes.

```powershell
Remove-ADUser -Identity <name>
```
Remove a user.

```powershell
Unlock-ADAccount -Identity <name>
```
Unlock a user account.

```powershell
Set-ADAccountPassword -Identity <name> -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "NewP@ssw0rdReset!" -Force)
```
Reset a user's password.

```powershell
Set-ADUser -Identity amasters -ChangePasswordAtLogon $true
```
Force password change at next logon.

## Groups / OUs
```powershell
New-ADOrganizationalUnit -Name "name" -Path "OU=folder,DC=domain,DC=local"
```
Create a new OU container.

```powershell
New-ADGroup -Name "name" -SamAccountName analysts -GroupCategory Security -GroupScope Global -DisplayName "Security Analysts" -Path "CN=Users,DC=domain,DC=local" -Description "..."
```
Create a new security group.

```powershell
Add-ADGroupMember -Identity 'group name' -Members 'ACepheus,OStarchaser,ACallisto'
```
Add members to a group.

## GPOs
```powershell
Copy-GPO -SourceName "GPO to copy" -TargetName "Name"
```
Copy a GPO into a new GPO.

```powershell
New-GPLink -Name "Security Analysts Control" -Target "OU=Security Analysts,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL" -LinkEnabled Yes
```
Link a GPO to an OU and enable it.

```powershell
Set-GPLink -Name "Security Analysts Control" -Target "OU=Security Analysts,..." -LinkEnabled Yes
```
Modify an existing GPO link.

## Computers
```powershell
Add-Computer -DomainName 'INLANEFREIGHT.LOCAL' -Credential 'INLANEFREIGHT\HTB-student_adm' -Restart
```
Join the local machine to the domain.

```powershell
Add-Computer -ComputerName 'name' -LocalCredential '.\localuser' -DomainName 'INLANEFREIGHT.LOCAL' -Credential 'INLANEFREIGHT\htb-student_adm' -Restart
```
Remotely join a computer to the domain.

```powershell
Get-ADComputer -Identity "name" -Properties * | select CN,CanonicalName,IPv4Address
```
View a computer's properties.
