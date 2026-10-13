#reversing #assembly #x86-64 #gdb #nasm

# Intro to Assembly (x86-64)

Related: [game-hacking](game-hacking.md) · [buffer-overflow](buffer-overflow.md) · [windbg](windbg.md)

## Contents
- [Registers (64-bit / purpose)](#registers-64-bit--purpose)
- [Assemble / Link / Disassemble](#assemble--link--disassemble)
- [GDB Workflow](#gdb-workflow)
- [Key Instructions](#key-instructions)

## Registers (64-bit / purpose)
| Register(s) | Role |
|---|---|
| `rax` | syscall number / return value |
| `rdi`, `rsi`, `rdx`, `rcx`, `r8`, `r9` | args 1–6 |
| `rbp` | base stack pointer |
| `rsp` | top stack pointer |
| `rip` | instruction pointer |

System V calling convention register roles.

## Assemble / Link / Disassemble
```bash
nasm -f elf64 helloWorld.s
```
Assemble to an object file.

```bash
ld -o fib fib.o -lc --dynamic-linker /lib64/ld-linux-x86-64.so.2
```
Link with libc (drop `-lc ...` for a static binary).

```bash
objdump -M intel -d helloWorld
```
Disassemble the .text section (Intel syntax).

```bash
objdump -sj .data helloWorld
```
Dump the .data section.

## GDB Workflow
```bash
gdb -q ./helloWorld
```
Open a binary (`info functions`, `info variables`).

```text
b *0x401000
```
Breakpoint at an address (`b _start` at a label).

```text
r
```
Run (`si` step instruction, `ni` step over, `c` continue).

```text
x/4xg $rip
```
Examine memory: `x/<count><format><size> $reg`.

```text
set $rdx=0x9
```
Set a register value.

```text
patch string 0x402000 "Patched!\x0a"
```
Patch bytes at an address.

## Key Instructions
```asm
mov rax, 1        ; load immediate
lea rax, [rsp+5]  ; load effective address (pointer)
xchg rax, rbx     ; swap two registers
```
Data movement.

| Instruction family | Examples |
|---|---|
| Data movement | `mov`, `lea`, `xchg` (shown above) |
| Arithmetic | `add`, `sub`, `inc`, `dec` |
| Comparison | `cmp` |
| Control flow | `call`, `ret`, `jmp` |

See [game-hacking](game-hacking.md).
