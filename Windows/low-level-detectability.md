#windows #malware #pe-format #windbg #anti-debug

# Windows Low-Level Detectability

Related: [process-injection](process-injection.md) · [kernel-telemetry](../Detection/kernel-telemetry.md) · [windbg](../Reversing/windbg.md)

## PE File Format Structures
| PE Entity | C Structure |
|---|---|
| DOS Header / Stub | `IMAGE_DOS_HEADER` |
| NT Headers | `IMAGE_NT_HEADERS` |
| File Header | `IMAGE_FILE_HEADER` |
| Optional Header | `IMAGE_OPTIONAL_HEADER` |
| Section Table | `IMAGE_SECTION_HEADER` |
| Export Directory | `IMAGE_EXPORT_DIRECTORY` |
| Import Directory | `IMAGE_IMPORT_DESCRIPTOR` |
| Resource Directory | `IMAGE_RESOURCE_DIRECTORY` |
| Exception Directory | `IMAGE_RUNTIME_FUNCTION_ENTRY` |
| Security Directory | `WIN_CERTIFICATE` |
| Base Relocation Table | `IMAGE_BASE_RELOCATION` |
| TLS Directory | `IMAGE_TLS_DIRECTORY` |
| Load Config Directory | `IMAGE_LOAD_CONFIG_DIRECTORY` |
| Import Address Table | `IMAGE_THUNK_DATA` |

## PE Sections
`.text` (code) · `.rdata` (read-only data) · `.data` (initialized globals) · `.bss` (uninitialized globals) · `.pdata` (SEH/unwind info) · `.rsrc` (icons/resources) · `.reloc` (relocation info) · `.edata`/`.idata` (export/import tables, when split from .rdata) · `.tls` (thread-local storage) · `.debug` (debug info) · `.loadcfg` (SafeSEH/DEP config).

## Win32 API Essentials (Malware-Relevant)
`CreateProcess`, `OpenProcess`, `VirtualAllocEx`, `WriteProcessMemory`, `CreateRemoteThread`, `ReadProcessMemory` — process creation/injection. `GetProcAddress`, `LoadLibrary` — dynamic API resolution (evasion). `RegCreateKey`, `RegSetValue` — persistence. `InternetOpen`, `InternetConnect`, `HttpSendRequest` — C2 communication. `CryptEncrypt`/`CryptDecrypt` — payload obfuscation. `CreateMutex` — single-instance marker. `SetWindowsHookEx` — keylogging/UI hooking. See [process-injection](process-injection.md) for the injection-specific subset.

## Hardware Breakpoints (DR0–DR3 / DR7)
`DR7` enable bits `L0..L3` (bits 0,2,4,6) toggle each debug register. Per-register `RWx`/`LENx` field combinations: `00`/`00` = execute, `01`/`00` = 1-byte write, `11`/`11` = 4-byte read/write. Anti-debug checks commonly read `DR7` via `GetThreadContext` to detect active hardware breakpoints.

## WinDbg Essentials
```windbg
!teb
!peb
dt <structure>
dt <structure> <address>
!process 0 0 powershell.exe
.process <address>
!dh
!dh -i
!dh -e
!dh -s
!load <extension.dll>
? <expr>
?? <expr>
poi(<addr>)
lm
lm m <module>
da <address>
dd <address>
dw <address>
db <address>
dqs <address>
u <address>
.fnent <address>
bp <address/symbol>
g
r <register>
~
~1s
k
~*k
```
Dump TEB/PEB and structures, find a process by name, dump a module's PE header/imports/exports/resources, load a debugger extension, evaluate expressions, dereference pointers, list/filter loaded modules, read memory in various widths (ASCII/DWORD/WORD/byte/QWORD-with-symbols), disassemble, resolve function entries, set breakpoints, resume execution, and inspect registers/threads/call stacks.
