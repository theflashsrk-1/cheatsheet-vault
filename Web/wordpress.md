#web #wordpress #wpscan #cms

# Hacking WordPress

Related: [common-applications](common-applications.md) · [fuzzing](fuzzing.md) · [metasploit](../Tools/metasploit.md)

## Recon
```bash
curl -I -X GET <url>
```
Inspect response headers (version hints, `/wp-` paths).

## WPScan
```bash
wpscan --url <url> -e ap
```
Enumerate all plugins.

```bash
wpscan --url <url> -e u
```
Enumerate users.

```bash
wpscan --url <url> -e ap --plugins-detection aggressive --api-token <TOKEN>
```
Aggressive plugin detection with vulnerability data.

## Exploitation
```bash
msfconsole
```
Launch Metasploit for WordPress plugin exploits. See [metasploit](../Tools/metasploit.md).

## Output Helpers
```bash
curl -s -X GET <url> | html2text
```
Convert HTML to readable text (pipe through `grep`, `jq` as needed).
