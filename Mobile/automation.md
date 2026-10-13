#mobile #android #mobsf #drozer #quark

# Android Pentest Automation

MobSF, Quark Engine, Drozer. Related: [static-analysis](static-analysis.md) · [dynamic-analysis](dynamic-analysis.md)

| Tool | Type | Best for |
|---|---|---|
| MobSF | Static + dynamic, web UI | All-in-one automated report (manifest, permissions, trackers, API hits) |
| Quark Engine | Static, CLI | Rule-based scoring of malicious/crime behaviors, CI-friendly |
| Drozer | Dynamic, on-device agent | Exercising exported components (activities/services/providers) at runtime |

## MobSF
```bash
docker run -it --rm -p 8000:8000 opensecurity/mobile-security-framework-mobsf:latest
```
Run MobSF locally (web UI on :8000).

## Quark Engine
```bash
pip3 install -U quark-engine
```
Install Quark.

```bash
quark -s -a myapp.apk -r quark-rules -t 100
```
Scan, showing only 100%-confidence rules.

```bash
quark -s -a myapp.apk -r quark-rules --permission
```
List requested permissions (`--list all` for methods).

```bash
quark -s -a myapp.apk -r quark-rules -t 100 -c
```
Focus on crime/malicious behaviors (`-w report` for an HTML report).

## Drozer
```bash
pipx install drozer
adb install drozer-agent.apk
```
Install the Drozer console and on-device agent.

```bash
adb forward tcp:31415 tcp:31415
drozer console connect 10.11.1.1:5001
```
Forward the port and connect.

```
dz> run app.package.list -f MyApplication
```
List installed packages (`app.package.info -a <pkg>` for details).
