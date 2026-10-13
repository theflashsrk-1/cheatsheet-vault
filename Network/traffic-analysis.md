#network #tcpdump #wireshark #tshark #pcap

# Network Traffic Analysis

tcpdump / tshark / Wireshark. Related: [pivoting](pivoting.md) · [ntlm-relay](../AD/ntlm-relay.md) · [tls-attacks](../Web/tls-attacks.md)

> Capturing needs root/sudo (binds the interface / promiscuous mode).

## tcpdump
```bash
tcpdump -D
```
List capturable interfaces.

```bash
tcpdump -i eth0 -w file.pcap
```
Capture on an interface and write to a file (`-r file.pcap` to read).

```bash
tcpdump -i eth0 host 10.10.10.5 and port 445
```
Filter by host and port.

```bash
tcpdump -i eth0 -nn -X
```
No name/port resolution; show packet contents in hex + ASCII.

```bash
tcpdump -r file.pcap -l | grep 'string'
```
Line-buffer output and grep for a pattern.

## tshark
```bash
tshark -i eth0 -f "host 10.10.10.5"
```
Capture with a libpcap capture filter (`-f`).

```bash
tshark -r file.pcap -Y "http.request" -x
```
Read a pcap, apply a display filter (`-Y`), show hex/ASCII (`-x`).

## Wireshark — Capture Filters (libpcap)
```
host x.x.x.x
net x.x.x.x/24
port 445
not port 80
host 192.168.1.1 and port 80
```
BPF capture-filter syntax.

## Wireshark — Display Filters
```
ip.addr == x.x.x.x
tcp.port == 445
dns / tcp / ftp / arp
tcp.stream eq 3
```
Display-filter syntax (follow a stream with `tcp.stream`).
