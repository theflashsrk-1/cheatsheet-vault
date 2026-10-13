#reversing #game-hacking #assembly #x86

# Game Hacking Fundamentals

x86 instruction reference for memory/code patching. Related: [assembly](assembly.md) · [windbg](windbg.md)

## Contents
- [Data Movement](#data-movement)
- [Arithmetic](#arithmetic)
- [Control Flow](#control-flow)
- [Conditional Jumps (after CMP)](#conditional-jumps-after-cmp)
- [Patching](#patching)

## Data Movement
```asm
MOV eax, ebx   ; move data
PUSH eax       ; push onto stack
POP eax        ; pop from stack
```

| Instruction | Example | Effect |
|---|---|---|
| `MOV` | `MOV eax, ebx` | move data |
| `PUSH` | `PUSH eax` | push onto stack |
| `POP` | `POP eax` | pop from stack |

## Arithmetic
```asm
ADD eax, ecx   ; add
SUB edx, eax   ; subtract
INC eax        ; +1
DEC ebx        ; -1
```

| Instruction | Example | Effect |
|---|---|---|
| `ADD` | `ADD eax, ecx` | add |
| `SUB` | `SUB edx, eax` | subtract |
| `INC` | `INC eax` | +1 |
| `DEC` | `DEC ebx` | -1 |

## Control Flow
```asm
CALL _functionName   ; call
RET                  ; return
JMP _label           ; unconditional jump
```

| Instruction | Example | Effect |
|---|---|---|
| `CALL` | `CALL _functionName` | call |
| `RET` | `RET` | return |
| `JMP` | `JMP _label` | unconditional jump |

## Conditional Jumps (after CMP)
```asm
CMP eax, ebx
JE  _label   ; equal      JNE _label  ; not equal
JG  _label   ; greater    JL  _label  ; less
JGE _label   ; >=         JLE _label  ; <=
```

| Instruction | Condition |
|---|---|
| `JE` | equal |
| `JNE` | not equal |
| `JG` | greater |
| `JL` | less |
| `JGE` | greater or equal |
| `JLE` | less or equal |

## Patching
```asm
NOP   ; no-op — overwrite checks/jumps to bypass logic (e.g. god-mode, no-reload)
```
Replace a conditional jump or decrement with NOPs to neutralize it.
