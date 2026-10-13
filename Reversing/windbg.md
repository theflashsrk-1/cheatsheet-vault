#reversing #windbg #debugging #ttd

# Dynamic Analysis with WinDbg

Related: [buffer-overflow](buffer-overflow.md) · [assembly](assembly.md) · [malicious-docs](../Detection/malicious-docs.md)

## Session / Symbols
```
.symfix <path>
```
Point symbols at the Microsoft symbol server (`.sympath` to view/set, `.reload /f` to force).

```
.attach 0n<PID>
```
Attach to a process by PID (`.detach`, `.restart`).

```
!analyze -v
```
Analyze the current exception/bugcheck (verbose).

## Disassembly / Symbols
```
uf <addr>
```
Disassemble a function (`u <addr>` for raw instructions).

```
x <module>!<pattern>
```
Examine matching symbols (e.g. `x *!*`).

## Execution Control
```
g
```
Continue (`g <addr>` to an address, `gu` until function return).

```
p
```
Step over (`t` to step into).

## Breakpoints
```
bp <addr>
```
Set a breakpoint (`bl` list, `bc <id>` clear).

```
bp <addr>;.printf "...";g
```
Conditional logging breakpoint that continues.

## Registers & Stack
```
r
```
Show registers (`r <reg>=<val>` to set).

```
k
```
Call stack (`kv` verbose, `~*k` all threads; `~` lists threads).

## Read / Write Memory
```
dd <addr>
```
Display dwords (`db` bytes, `dq` qwords, `du` Unicode string, `dps` pointers+symbols).

```
ed <addr> <value>
```
Write a dword (`eb` byte, `eq` qword).

```
dt [-rN] <type> <addr>
```
Display a structure/type at an address.

## Modules / PE
```
lm
```
List loaded modules (`lm f` detailed).

```
!dh -i <BaseAddr>
```
Show a module's import table (`-e` for exports).

## Time Travel Debugging
```
g-
```
Run backward (`p-`/`t-` step backward).

```
dx @$cursession.TTD.Calls
```
Query recorded calls (`.Memory`, `.Events` for more TTD data).
