#codereview #secrets #git #osint

# Git & Secrets Hunting

Related: [supply-chain](supply-chain.md) · [cicd-attacks](cicd-attacks.md) · [corporate-recon](../OSINT/corporate-recon.md)

## Exposed .git Directory on a Web Server
```bash
curl -s https://target.com/.git/config
```
If this returns a real git config instead of a 404, the full repo history is likely downloadable.
```bash
pip3 install git-dumper
git-dumper https://target.com/.git/ ./loot
```
Reconstruct the full repository (objects, refs, history) from an exposed `.git` directory — then diff/grep history for secrets that were later removed.

## Scanning a Repo (Local or Cloned)
```bash
pip3 install trufflehog3 2>/dev/null || true
trufflehog filesystem /path/to/repo --results=verified
trufflehog git file:///path/to/repo --since-commit HEAD~50
```
`filesystem` scans a working tree; `git` walks commit history (not just the current checkout) — history matters because secrets are often added then "removed" in a later commit while still sitting in the object database. `--results=verified` filters to secrets TruffleHog actually confirmed live against the provider's API (cuts noise dramatically).

```bash
gitleaks detect --source . --report-format json --report-path gitleaks-report.json
gitleaks detect --source . --log-opts="HEAD~20..HEAD" --redact
```
Rule-based secret scan of a repo; `--log-opts` passes straight through to `git log` to scope the commit range, `--redact` hides the actual secret value in output (useful when reporting). Newer gitleaks versions split this into `gitleaks git`/`gitleaks dir`/`gitleaks stdin` — check `gitleaks --help` on your installed version.

## GitHub-Wide / Org-Wide Scanning
```bash
docker run -it -v "$PWD:/pwd" trufflesecurity/trufflehog:latest github --org=target-org
docker run -it -v "$PWD:/pwd" trufflesecurity/trufflehog:latest github --repo=https://github.com/org/repo.git
```
Scan an entire GitHub org or a single public repo's full history without cloning it yourself first.

## GitHub Dorking (Manual)
```text
org:target-org password
org:target-org "BEGIN RSA PRIVATE KEY"
org:target-org extension:env DB_PASSWORD
org:target-org extension:yml aws_secret_access_key
filename:.npmrc _authToken
```
Search qualifiers for GitHub's native code search — catches secrets in repos a scanner wasn't pointed at, including forks and gists.

## npm / pip / Package Metadata Leaks
```bash
npm info <package> maintainers
pip download <package> --no-deps -d /tmp/pkg && tar -tf /tmp/pkg/*.tar.gz
```
Published packages occasionally ship `.env`, `.npmrc`, or build-config files accidentally included in the package tarball — worth a quick look during supply-chain assessments. See [supply-chain](supply-chain.md).
