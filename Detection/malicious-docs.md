#detection #malware #pdf #office #rtf

# Malicious Document Analysis

PDF/Office/RTF triage + shellcode emulation. Related: [windbg](../Reversing/windbg.md) · [malware-analysis](../Mobile/malware-analysis.md)

## General
```
trid <file>
```
Identify the file type.

```cmd
scdbg.exe /f c:\temp\rtf_shellcode.sc /foff 372
```
Emulate shellcode (at an offset) with scdbg.

```cmd
XORSearch.exe -W c:\temp\rtf_shellcode.sc
```
Find the shellcode entrypoint.

```bash
speakeasy -t rtf_shellcode.sc -a x86 -r --raw_offset 372
```
Emulate shellcode with speakeasy.

## PDF
```bash
python pdfid.py <pdf> -a
```
Summarize object types (JS, OpenAction, etc.).

```bash
python pdf-parser.py <pdf> -s /URI
```
Search objects (`--search=<kw>`, `--objstm` to parse streams).

```bash
python pdf-parser.py -o <obj_id> -d image.jpeg <pdf>
```
Dump an object to a file.

## Office (OLE / VBA)
```bash
python oleid.py <office-document>
```
Summarize suspicious indicators.

```bash
python olevba.py <office-document>
```
Extract and analyze VBA macros.

```bash
python oleobj.py <office-document>
```
Extract external relationships (remote templates). See [ntlm-relay](../AD/ntlm-relay.md).

## RTF
```bash
python rtfdump.py <RTF-document>
```
Dump RTF structure level-wise.

```bash
python rtfdump.py <RTF-document> --select 4 --hexdecode --dump > rtf_shellcode.sc
```
Extract an embedded shellcode object.
