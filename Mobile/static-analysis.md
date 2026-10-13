#mobile #android #apktool #jadx #reversing

# Android Static Analysis

Related: [android-fundamentals](android-fundamentals.md) · [dynamic-analysis](dynamic-analysis.md) · [automation](automation.md) · [whitebox](../CodeReview/whitebox.md)

## Extract the APK
```bash
adb shell pm path com.example.myapp
adb pull /data/app/com.example.myapp-1/base.apk .
```
Locate and pull an installed APK off the device.

## Disassemble / Decompile
```bash
apktool d myapp.apk
```
Decode an APK to smali + resources.

```bash
jadx-gui /full/path/to/myapp.apk
```
Decompile to Java for review.

## React Native / Hermes
```bash
js-beautify myapp/assets/index.android.bundle -o beautified.js
```
Beautify a minified JS bundle.

```bash
hbc-decompiler index.android.bundle output.js
```
Decompile Hermes bytecode (hermes-dec).

## Deobfuscation
```bash
python -m paranoid_deobfuscator -v myapp
```
Deobfuscate Paranoid/LSParanoid-protected apps.

## Native / Xamarin
```bash
python Xamarin_XALZ_decompress.py myapp/unknown/assemblies/Myapp.dll out.dll
```
Decompress XALZ Xamarin assemblies (`pyxamstore unpack` for .blob stores).

## Patch & Re-sign
```bash
apktool b myapp
```
Rebuild the APK from decoded sources.

```bash
keytool -genkey -keystore key.keystore -validity 1000 -keyalg RSA -alias john
```
Generate a signing key.

```bash
zipalign -p -f -v 4 myapp/dist/myapp.apk myapp_aligned.apk
apksigner sign --ks key.keystore myapp_aligned.apk
```
Align and sign the rebuilt APK (then reinstall via adb).
