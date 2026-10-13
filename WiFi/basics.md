#wifi #aircrack-ng #monitor-mode #wpa-supplicant

# Wi-Fi Pentesting Basics

Interface modes, aircrack-ng essentials, connecting. Related: [tools](tools.md) · [wep](wep.md) · [wpa-wpa2](wpa-wpa2.md) · [password-cracking](password-cracking.md)

## Contents
- [Interface Modes](#interface-modes)
- [airmon-ng / airodump-ng](#airmon-ng--airodump-ng)
- [aireplay-ng](#aireplay-ng)
- [Decrypt / Crack](#decrypt--crack)
- [Connecting (wpa_supplicant)](#connecting-wpa_supplicant)
- [Bypasses](#bypasses)

## Interface Modes
```bash
sudo iw reg set US
```
Set the regulatory region (unlocks channels/power).

```bash
sudo ifconfig wlan0 down; sudo iw wlan0 set monitor control; sudo ifconfig wlan0 up
```
Set the interface to monitor mode.

```bash
iwlist wlan0 scan | grep 'Cell\|Quality\|ESSID\|IEEE'
```
Scan available networks (managed mode).

```bash
sudo ifconfig wlan0 down; sudo iwconfig wlan0 channel 64; sudo ifconfig wlan0 up
```
Change channel (`freq "5.52G"` for frequency).

## airmon-ng / airodump-ng
```bash
sudo airmon-ng start wlan0 11
```
Enable monitor mode on a specific channel.

```bash
sudo airodump-ng wlan0mon -w HTB
```
Scan and save capture to files (`-c 11` channel, `--band a` for 5 GHz).

```bash
airgraph-ng -i HTB-01.csv -g CAPR -o HTB_CAPR.png
```
Graph client↔AP relationships (`-g CPG` for probe graph).

## aireplay-ng
```bash
sudo aireplay-ng --test wlan0mon
```
Test packet-injection capability.

```bash
aireplay-ng -0 5 -a 00:14:6C:7A:41:81 -c 00:0F:B5:32:31:31 wlan0mon
```
Deauthenticate a client (capture handshake). See [wpa-wpa2](wpa-wpa2.md).

## Decrypt / Crack
```bash
airdecap-ng -w 1234567890ABCDEF HTB-01.cap
```
Decrypt a WEP capture with a known key.

```bash
aircrack-ng HTB.pcap -w /opt/wordlist.txt
```
Crack WPA from a capture (`-K HTB.ivs` for WEP Korek). See [password-cracking](password-cracking.md).

## Connecting (wpa_supplicant)
```ini
network={ ssid="HackMe" psk="password123" }
```
WPA-Personal config, then `wpa_supplicant -c wpa.conf -i wlan0` (use `key_mgmt=WPA-EAP` + identity/password for Enterprise).

## Bypasses
```bash
mdk3 wlan0mon p -f /opt/wordlist.txt -t D2:A3:32:13:29:D5
```
Brute-force a hidden SSID from a wordlist (`-b u` for all values).[^1]

```bash
sudo macchanger wlan0 -m 3E:48:72:B7:62:2A
```
Spoof the interface MAC (bypass MAC filtering).

[^1]: `mdk3` is unmaintained upstream; if it's unavailable on current distros, the community `mdk4` fork provides the same attack modes.
