#detection #forensics #dfir #registry #regripper

# User Behavior Forensics (Windows DFIR)

Registry artifacts + EZ-Tools/RegRipper parsing. Related: [linux-forensics](linux-forensics.md) · [credential-theft](../Windows/credential-theft.md)

## Key Registry Artifacts
```
Shellbags   HKCU\...\Shell\BagMRU            (folder access)
UserAssist  HKCU\...\Explorer\UserAssist     (GUI program execution)
RecentDocs  HKCU\...\Explorer\RecentDocs     (opened documents)
RunMRU      HKCU\...\Explorer\RunMRU         (Run dialog history)
TypedPaths  HKCU\...\Explorer\TypedPaths     (Explorer address bar)
USBSTOR     HKLM\SYSTEM\...\USBSTOR          (connected USB devices)
```
Common user-activity keys to examine.

## RegRipper
```cmd
rip.exe -l
```
List all RegRipper plugins.

```cmd
rip.exe -r NTUSER.DAT -p userassist_tln
```
Run a plugin against a hive (`-p shellbags`, `recentdocs`, `runmru`, `usbstor`, etc.).

## EZ-Tools
```cmd
JLECmd.exe -d "...\Recent" --csv out
```
Parse JumpLists to CSV.

```cmd
LECmd.exe -f passwords.lnk
```
Parse LNK files (`-d <dir>` for many).

```cmd
WxTCmd.exe -f ActivitiesCache.db --csv C:\tmp
```
Parse the Windows Timeline (ActivitiesCache.db).

```cmd
RegistryExplorer.exe
```
GUI registry hive browser (EZ-Tools).

## Other Parsers
```cmd
py stickyparser.py -p plum.sqlite -d C:\Temp
```
Parse Sticky Notes database.
