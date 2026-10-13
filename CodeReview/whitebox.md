#codereview #whitebox #sast #prototype-pollution

# Whitebox Pentesting

Source-assisted testing: review → local test → PoC → patch. Related: [whitebox-cmdi](whitebox-cmdi.md) · [secure-coding-js](secure-coding-js.md) · [deserialization](../Web/deserialization.md) · [command-injection](../Web/command-injection.md)

## Dangerous Sink Functions (grep targets)
```
JS/Node: eval, exec, child_process.exec/spawn, Function, setTimeout(string)
Python:  eval, exec, os.system, subprocess.run/Popen, os.popen
PHP:     eval, system, shell_exec, passthru, proc_open, popen
C/C++:   execlp, execvp, system, popen
C#/Java: Process.Start, Runtime.getRuntime().exec
```
Code/command-injection sinks to hunt during review.

## Payload Development Rules
```
1. Comment out the rest of the code
2. Keep quotes/parentheses/braces balanced
3. Leave a syntactically valid function
```
Keep the injected payload from breaking the surrounding code.

## Output Retrieval (blind)
```
console log | reverse shell | DNS/ping exfil | write to file | HTTP response | sleep/boolean inference
```
Ways to read command output when there's no direct echo.

## Prototype Pollution (JS)
```javascript
__proto__.status
```
Pollute via `__proto__` / `constructor.prototype`. Prevent with `Object.freeze()` / `Object.create(null)`. See [secure-coding-js](secure-coding-js.md).

## Type Juggling (PHP)
```php
if ($a == $b)   // loose comparison — exploitable
```
Loose `==`/`!=` cause unexpected matches (e.g. magic hashes). Fix: use `===`/`!==`.

## Timing / Race Conditions
```
Timing: measurable task time leaks info -> make paths constant-time
Race:   assume multi-threaded; use file/DB locks on critical ops
```
Concurrency and side-channel review checklist.
