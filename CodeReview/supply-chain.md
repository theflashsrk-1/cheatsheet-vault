#codereview #supply-chain #cicd #dependencies

# Supply Chain Attacks (Reference)

Conceptual module — no tooling commands. Related: [whitebox](whitebox.md) · [deserialization](../Web/deserialization.md)

## Attack Lifecycle
```
1 Target ID → 2 Supply-chain mapping → 3 Vuln discovery → 4 Initial exploitation
→ 5 Lateral movement → 6 Compromise → 7 Exfil/sabotage → 8 Evasion & persistence
```
Stages of a supply-chain compromise.

## Software Supply Chain Weak Points
```
- Development: malicious code / backdoors, compromised dev tools
- Dependencies: dependency confusion, compromised libraries (SCA)
- Version control: unauthorized commits
- Build/CI-CD: poisoned pipeline injecting code at build
- Deployment/Distribution: replacing legit artifacts (app stores, download sites)
```
Where software supply chains get attacked.

## Hardware Supply Chain Weak Points
```
Raw materials → component mfg (hardware trojans) → assembly (malicious firmware)
→ QC (falsified tests) → packaging (tamper) → logistics → retail (POS tampering)
```
Where hardware supply chains get attacked.

## Insider Threat Types
```
Malicious insider | Negligent insider | Infiltrator (poses as employee/contractor)
```
Insider categories that enable supply-chain compromise.
