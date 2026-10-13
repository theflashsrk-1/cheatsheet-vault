#detection #forensics #linux #volatility #auditd

# Linux Forensics

Logs, auditd, journald, and memory forensics. Related: [siem](siem.md) · [user-behavior-forensics](user-behavior-forensics.md) · [process-injection](../Windows/process-injection.md)

## Logs
```bash
tail -n 10 /var/log/syslog
```
Recent syslog entries (`dmesg` for kernel logs).

```bash
journalctl --utc --since "2023-10-15 18:00:00" --until "2023-10-15 19:00:00" -u httpd.service
```
Filter journald by time + unit (`_PID=`, executable, `-p err` also work; `-b -1` prior boot).

## auditd
```bash
ausearch --syscall EXECVE -if audit.log
```
Search audit logs for process execution.

```bash
ausearch -if audit.log -m ADD_USER -m DEL_USER -m ADD_GROUP -m ROLE_ASSIGN -i
```
Search for user/group management events.

```bash
aureport -if audit.log
```
Generate a file-access report (`auditctl -s` for daemon status).

```bash
python3 zircolite.py --events audit.log --auditd --ruleset rules/rules_linux.json --csv -o out.csv
```
Apply SIGMA rules to auditd logs (Zircolite).

## Memory Forensics (Volatility 3)
```bash
sudo ./avml memdump.mem
```
Acquire a volatile memory dump (AVML).

```bash
python3 ~/volatility3/vol.py -q -f server-dump linux.pstree
```
Process tree from memory (`linux.pslist`, `linux.psaux` variants).

```bash
python3 ~/volatility3/vol.py -q -f server-dump linux.bash.Bash
```
Recover bash history from memory.

```bash
python3 ~/volatility3/vol.py -q -f server-dump linux.malfind.Malfind --pid 28645
```
Find injected/hidden code in a process. See [process-injection](../Windows/process-injection.md).

```bash
python3 ~/volatility3/vol.py -q -f server-dump linux.check_syscall
```
Detect syscall-table hooking (rootkits) (`linux.lsmod` for modules).

```bash
python3 ~/volatility3/vol.py -q -f server-dump linux.proc.Maps --dump --pid 910
```
Dump a process's memory maps (then `strings -a <dump>`).
