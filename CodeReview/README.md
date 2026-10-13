# CodeReview (Whitebox)

Source-assisted testing notes — reviewing application code, CI/CD pipelines, and dependency chains to find and confirm vulnerabilities before they ever hit a black-box scan.

| Note | Covers |
|---|---|
| [whitebox](whitebox.md) | SAST workflow — dangerous sink functions to grep for, review → local test → PoC → patch |
| [whitebox-cmdi](whitebox-cmdi.md) | Local debugging and exploitation of a command-injection flaw in a Node.js app |
| [csharp](csharp.md) | C# language reference — syntax basics and security-relevant sinks |
| [secure-coding-js](secure-coding-js.md) | JavaScript deobfuscation and secure-coding review tooling |
| [supply-chain](supply-chain.md) | Supply-chain attack lifecycle and software supply-chain weak points (conceptual) |
| [git-secrets](git-secrets.md) | Hunting exposed `.git` directories and secrets in repo history |
| [cicd-attacks](cicd-attacks.md) | GitHub Actions, Jenkins, and GitLab CI pipeline attacks — secrets exfiltration, `pull_request_target` misuse, runner abuse |
