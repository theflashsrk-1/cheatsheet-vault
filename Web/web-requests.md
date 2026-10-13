#web #curl #http #api

# Web Requests (cURL / APIs)

Related: [web-proxies](web-proxies.md) · [ffuf](ffuf.md) · [footprinting](../Network/footprinting.md)

## cURL Basics
```bash
curl -s -O inlanefreight.com/index.html
```
Download a file (keep its name).

```bash
curl -k https://inlanefreight.com
```
Skip TLS certificate validation.

```bash
curl -i https://www.inlanefreight.com
```
Print response headers + body (`-I` for headers only, `-v` for full detail).

```bash
curl https://www.inlanefreight.com -A 'Mozilla/5.0'
```
Set the User-Agent.

## Auth & Headers
```bash
curl -u admin:admin http://<SERVER_IP>:<PORT>/
```
HTTP basic auth (also `http://admin:admin@host/`).

```bash
curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/
```
Set an arbitrary request header.

```bash
curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
```
Set request cookies.

## POST Data
```bash
curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/
```
Send a form-encoded POST.

```bash
curl -X POST -d '{"search":"london"}' -H 'Content-Type: application/json' http://<SERVER_IP>:<PORT>/search.php
```
Send a JSON POST.

## REST APIs
```bash
curl -s http://<SERVER_IP>:<PORT>/api.php/city/ | jq
```
Read all entries and pretty-print.

```bash
curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
```
Create an entry.

```bash
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london -d '{"city_name":"New_HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
```
Update an entry.

```bash
curl -X DELETE http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City
```
Delete an entry.
