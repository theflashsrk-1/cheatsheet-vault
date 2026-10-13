# Linux

Linux-side privilege escalation, macOS-as-a-target fundamentals, and the ELF/process-injection internals needed to understand (and detect) injection on Linux.

| Note | Covers |
|---|---|
| [fundamentals](fundamentals.md) | Core command reference — identity, package management, files, text processing, permissions, processes/services, networking |
| [privesc](privesc.md) | Linux privilege escalation: enumeration, cron/PATH abuse, SUID/sudo/GTFOBins, LD_PRELOAD, LXD/LXC, NFS no_root_squash, shared tmux/screen hijacking |
| [macos](macos.md) | macOS fundamentals — files/permissions, networking, Homebrew, shell setup |
| [process-injection-detection](process-injection-detection.md) | ELF internals, GDB/GOT-PLT inspection, `/proc/[pid]/` reference, ptrace primitives, auditctl detection rules |
