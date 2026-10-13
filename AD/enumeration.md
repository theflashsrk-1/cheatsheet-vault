#ad #enumeration #powerview #bloodhound #ldap

# AD Enumeration

Related: [kerberoasting](kerberoasting.md) · [dacl-attacks](dacl-attacks.md) · [trust-attacks](trust-attacks.md) · [netexec](NetExec.md) · [nmap](../Network/nmap.md)

## RSAT / Module Setup
```powershell
Get-WindowsCapability -Name RSAT* -Online | Select-Object -Property Name, State
```
Check if RSAT tools are installed.

```powershell
Get-WindowsCapability -Name RSAT* -Online | Add-WindowsCapability -Online
```
Install all RSAT tools.

```powershell
Import-Module ActiveDirectory
```
Import the Active Directory PowerShell module.

```powershell
runas /netonly /user:htb.local\jackie.may powershell
```
Run a utility as another user.

## LDAP Queries (ActiveDirectory module)
```powershell
Get-ADGroup -Identity "<GROUP NAME>" -Properties *
```
Get information about an AD group.

```powershell
Get-ADObject -LDAPFilter '(objectClass=group)' | select cn
```
Return all AD groups.

```powershell
Get-ADUser -LDAPFilter '(userAccountControl:1.2.840.113556.1.4.803:=2)' | select name
```
List disabled users (UAC bit 2).

```powershell
(Get-ADUser -SearchBase "OU=Employees,DC=INLANEFREIGHT,DC=LOCAL" -Filter *).count
```
Count all users in an OU.

```powershell
Get-ADComputer -Filter "DNSHostName -like 'SQL*'"
```
Get hostnames containing "SQL".

```powershell
Get-ADGroup -Filter "adminCount -eq 1" | select Name
```
Get all protected/administrative groups.

```powershell
Get-ADUser -Filter {adminCount -eq '1' -and DoesNotRequirePreAuth -eq 'True'}
```
Find admin users that don't require Kerberos pre-auth. See [kerberoasting](kerberoasting.md).

```powershell
Get-ADUser -Filter {adminCount -gt 0} -Properties admincount,useraccountcontrol
```
Enumerate UAC values for admin users.

```powershell
Get-WmiObject -Class win32_group -Filter "Domain='INLANEFREIGHT'"
```
Get AD groups via WMI.

```powershell
([adsisearcher]"(&(objectClass=Computer))").FindAll()
```
Use ADSI to search for all computers.

```cmd
get-ciminstance win32_product | fl
```
Query installed software.

## PowerView / SharpView
```powershell
Get-DomainPolicy
```
View the domain password policy.

```powershell
Get-DomainUser | ConvertFrom-UACValue -showall
```
List all UAC values for a user.

```powershell
.\SharpView.exe ConvertTo-SID -Name sally.jones
```
Convert a username to a SID.

```powershell
.\SharpView.exe Convert-ADName -ObjectName S-1-5-21-2974783224-3764228556-2640795941-1724
```
Convert a SID to a username.

```powershell
.\SharpView.exe Get-Domain
```
View information about the current domain.

```powershell
.\SharpView.exe Get-DomainOU
```
List all OUs.

```powershell
.\SharpView.exe Get-DomainUser -KerberosPreauthNotRequired
```
Find ASREPRoastable users. See [kerberoasting](kerberoasting.md).

```powershell
.\SharpView.exe Get-DomainUser -SPN
```
Find users with SPNs set (Kerberoastable). See [kerberoasting](kerberoasting.md).

```powershell
Get-DomainComputer
```
List domain computers.

```powershell
.\SharpView.exe Get-DomainGPO | findstr displayname
```
List all GPO names.

```powershell
Get-DomainGPO -ComputerIdentity WS01
```
List GPOs applied to a specific host.

```powershell
Test-AdminAccess -ComputerName SQL01
```
Test local admin access on a remote host.

```powershell
.\SharpView.exe Get-NetShare -ComputerName SQL01
```
Enumerate open shares on a remote computer.

```powershell
Find-DomainUserLocation
```
Find machines where domain users are logged in.

```powershell
Get-DomainUser -Properties samaccountname,description | Where {$_.description -ne $null}
```
Find non-blank user description fields (often hold creds).

```powershell
.\SharpView.exe Get-DomainGroupMember -Identity 'Help Desk'
```
Get members of a domain group.

```powershell
.\SharpView.exe Get-DomainGroup -AdminCount
```
List protected groups.

```powershell
Get-NetLocalGroup -ComputerName WS01
```
Get local groups on a host.

```powershell
.\SharpView.exe Get-NetLocalGroupMember -ComputerName WS01
```
Get members of a local group.

```powershell
.\SharpView.exe Get-DomainComputer -Unconstrained
```
Find computers with unconstrained delegation. See [kerberos-delegation](kerberos-delegation.md).

```powershell
Get-DomainComputer -TrustedToAuth
```
Find computers with constrained delegation. See [kerberos-delegation](kerberos-delegation.md).

```powershell
Get-DomainObjectAcl -Identity harry.jones
```
Enumerate ACLs on a user. See [dacl-attacks](dacl-attacks.md).

```powershell
Find-InterestingDomainAcl
```
Find objects with modification rights over non-built-in objects.

```powershell
Get-PathAcl "\\SQL01\DB_backups"
```
Find ACLs set on a directory.

```cmd
gpresult /r /S WS01
```
Report all GPOs applied to a host.

```powershell
Get-DomainTrustMapping
```
Enumerate trusts for our domain / reachable domains. See [trust-attacks](trust-attacks.md).

## BloodHound Collection
```powershell
.\SharpHound.exe -c all --zipfilename inlanefreight_bloodhound
```
Run the SharpHound C# ingestor.

```powershell
Invoke-BloodHound -CollectionMethod all -ZipFileName ilfreight_bloodhound
```
Run the SharpHound PowerShell ingestor.

```bash
bloodhound-python -d <DOMAIN> -u <user> -p <password> -ns <DC_IP> -dc <DC> -gc <GC> -c All
```
Run bloodhound-python (Linux).

```bash
xfreerdp /v:10.129.2.43 /u:htb-student /drive:data,/tmp
```
RDP with drive redirection to move collection data off target.
