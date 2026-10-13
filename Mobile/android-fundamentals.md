#mobile #android #adb

# Android Fundamentals (ADB)

Related: [static-analysis](static-analysis.md) · [dynamic-analysis](dynamic-analysis.md) · [automation](automation.md)

## Setup
```bash
apt-get install adb
```
Install ADB (Linux; `brew install android-platform-tools` on macOS, `scoop install adb` on Windows).

```bash
adb start-server
```
Start the ADB server.

## Devices & Shell
```bash
adb devices
```
List connected devices/AVDs.

```bash
adb shell
```
Open an interactive shell (`adb root` to restart as root[^1]).

## Apps & Files
```bash
adb install myapp.apk
```
Install an APK.

```bash
adb push ./myapp.apk /sdcard/Download/
```
Push a file to the device.

```bash
adb pull /sdcard/Download/myapp.apk .
```
Pull a file from the device.

[^1]: Only works on `userdebug`/`eng` builds or an already-rooted device — retail/production builds reject `adb root`.
