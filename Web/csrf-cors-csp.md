#web #csrf #cors #csp #reference

# CSRF / CORS / CSP Reference

Defenses and misconfigurations (mostly conceptual — reference for exploitation). Related: [xss](xss.md) · [auth-attacks](auth-attacks.md)

## CSRF Defenses
```
CSRF token: unpredictable random value, bound to the user session, submitted in the form
```
Primary defense; absence/weak binding = CSRF.

```
SameSite cookie: none | lax (default) | strict
```
`strict` blocks all cross-origin; `lax` allows top-level GET navigations.

## CORS Headers
```
Access-Control-Allow-Origin: <origin>
Access-Control-Allow-Credentials: true
```
`ACAO` reflecting the request Origin **with** `credentials: true` = exploitable.

## CORS Misconfigurations
```
1. Arbitrary Origin Reflection in Access-Control-Allow-Origin
2. Improper Origin whitelist (substring/suffix match)
3. Trusted "null" Origin
```
Common exploitable ACAO patterns.

## Simple vs Preflighted Requests
```
Simple: GET/HEAD, or POST with Content-Type in {application/x-www-form-urlencoded, multipart/form-data, text/plain}, no custom headers
Preflighted: everything else (OPTIONS sent first)
```
Determines whether a preflight (OPTIONS) is required.

## CSP Directives
```
script-src | style-src | img-src | object-src | connect-src | default-src | frame-ancestors | form-action
```
Key directives; `frame-ancestors` prevents clickjacking, `form-action` restricts form targets. See [xss](xss.md).
