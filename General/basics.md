#general #tmux #vim #terminal

# Terminal Basics (tmux / vim)

The rest of "Getting Started" / "Pentest in a Nutshell" content (nmap, gobuster, msfconsole, shells, NetExec, hydra, xfreerdp, linpeas, etc.) is already covered — see [nmap](../Network/nmap.md), [web-recon](../Web/web-recon.md), [metasploit](../Tools/metasploit.md), [shells-payloads](../Tools/shells-payloads.md), [netexec](../AD/NetExec.md), [login-brute-forcing](../Passwords/login-brute-forcing.md), [privesc](../Linux/privesc.md), [privesc](../Windows/privesc.md).

## tmux
```bash
tmux
```
Start a new tmux session.

| Keys | Action |
|---|---|
| `ctrl+b` | Default tmux prefix key |
| `prefix c` | New window |
| `prefix 1` | Switch to window 1 |
| `prefix shift+%` | Split pane vertically |
| `prefix shift+"` | Split pane horizontally |
| `prefix ->` | Switch to the pane on the right |

## vim
```text
vim file
esc then i
esc
```
Open a file · enter insert mode · back to normal mode.

| Command | Action |
|---|---|
| `x` | Cut character |
| `dw` | Cut word |
| `dd` | Cut full line |
| `yw` | Copy word |
| `yy` | Copy full line |
| `p` | Paste |
| `:1` | Go to line 1 |
| `:w` | Write (save) |
| `:q` | Quit |
| `:q!` | Quit without saving |
| `:wq` | Write and quit |
