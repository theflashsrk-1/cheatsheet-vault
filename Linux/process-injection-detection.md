#linux #process-injection #elf #gdb #ptrace #auditd

# Linux Process Injection & Detection

Related: [process-injection](../Windows/process-injection.md) · [privesc](privesc.md) · [atomic-red-team](../Detection/atomic-red-team.md)

## Contents
- [Build / Linking](#build--linking)
- [GDB](#gdb)
- [/proc/[pid]/ Reference](#procpid-reference)
- [x86-64 Linux Calling Convention](#x86-64-linux-calling-convention)
- [ptrace Options](#ptrace-options)
- [auditctl (Detection)](#auditctl-detection)

## Build / Linking
```bash
gcc program.c -o program
```
Default build (linker behavior may be eager-bind).

```bash
gcc program.c -o program -Wl,-z,lazy
```
Force lazy symbol resolution (PLT stays active longer — relevant to GOT/PLT hijacking).

```bash
readelf -d program | grep "FLAGS)"
```
Show dynamic flags (e.g. check for `BIND_NOW`).

```bash
LD_DEBUG=bindings ./program
```
Trace dynamic-linker symbol-binding activity at runtime.

```bash
objdump -d -M intel -s -j .plt ./program
```
Disassemble and dump the `.plt` section (swap `.plt` for `.plt.sec`, `.got`, `.plt.got` as needed).

## GDB
```bash
gdb -q ./program
```
Start gdb quietly.

```text
set disassembly-flavor intel
break puts@plt
run
disas $pc
si
x/20i $pc
x/gx <addr>
info sym <addr>
break _dl_runtime_resolve_xsavec
continue
finish
info proc mappings
info files
```
Intel-syntax disassembly, break on PLT symbols or the runtime resolver, step/inspect GOT entries, and map memory back to loaded files.

## /proc/[pid]/ Reference
| Entry | Details |
|---|---|
| `cmdline` | Full command line, NUL-separated |
| `cwd`, `exe`, `root` | Symlinks to CWD / executed binary / chroot-aware root |
| `fd/`, `fdinfo/` | Open file descriptors and their flags/position |
| `maps`, `smaps` | Virtual memory map / detailed per-VMA memory stats |
| `mem` | Raw process memory (read/write via ptrace or root) |
| `ns/` | Namespace handles (mnt, pid, uts, net, user…) |
| `status`, `stat`, `statm` | Human-readable / raw / memory summary stats |
| `task/` | Per-thread subdirectories |

## x86-64 Linux Calling Convention
Argument order: `RDI`, `RSI`, `RDX`, `RCX`, `R8`, `R9`, then stack.

## ptrace Options
| Category | Options |
|---|---|
| Attach / detach | `PTRACE_TRACEME`, `PTRACE_ATTACH`, `PTRACE_SEIZE`, `PTRACE_DETACH` |
| Execution control | `PTRACE_CONT`, `PTRACE_SINGLESTEP`, `PTRACE_SYSCALL`, `PTRACE_INTERRUPT`, `PTRACE_LISTEN` |
| Register / FP register access | `PTRACE_GETREGS`/`PTRACE_SETREGS`, `PTRACE_GETFPREGS`/`PTRACE_SETFPREGS`, `PTRACE_GETREGSET`/`PTRACE_SETREGSET` |
| Signals / events | `PTRACE_GETEVENTMSG`, `PTRACE_GETSIGINFO`/`PTRACE_SETSIGINFO` |
| Config / introspection | `PTRACE_SETOPTIONS`, `PTRACE_GET_SYSCALL_INFO` |

Core primitives behind process injection/debugging on Linux (`ptrace(2)`).

## auditctl (Detection)
```bash
auditctl -D
auditctl -w /etc/ld.so.preload -p wa -k ld_preload_hijack
auditctl -a always,exit -F arch=b64 -S execve -F uid!=0
auditctl -b 8192 -f 1 -e 2
```

> [!WARNING]
> `-D` clears **all** existing audit rules, not just the ones you're about to add — save `auditctl -l` output first if other rules are already in place.

| Flags | Purpose |
|---|---|
| `-D` | Clear existing rules |
| `-w` / `-p` / `-k` | Watch a file for write/append and tag the rule |
| `-a` / `-S` / `-F` | Append a syscall rule with filters |
| `-b` / `-f` / `-e` | Tune backlog size, failure mode, and lock the config |

Use `ausyscall --dump` to find syscall names.
