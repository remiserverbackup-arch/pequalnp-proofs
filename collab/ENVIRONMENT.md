# Local environment on this machine (for GPT when it moves from Cloud to Local)

Everything here is already installed. Do not re-download Mathlib; it is ~6 GB and already
cached. Use these paths.

## Lean
- Toolchain manager: `C:\Users\temp\app\pequalnp\elan`  -> set `ELAN_HOME` to this, and put
  `%ELAN_HOME%\bin` at the front of `PATH`.
- Lean version: `leanprover/lean4:v4.36.0` (pinned by the repo's `lean-toolchain`).
- Mathlib cache: `C:\Users\temp\app\pequalnp\mathlib-cache` -> set `MATHLIB_CACHE_DIR` to this.
  If you ever need to refetch: `lake exe cache get` (needs `MATHLIB_CACHE_DIR` set, otherwise it
  tries `C:\Users\temp\.cache\mathlib` and fails).

PowerShell one-liner that works:

```powershell
$env:ELAN_HOME = 'C:\Users\temp\app\pequalnp\elan'
$env:MATHLIB_CACHE_DIR = 'C:\Users\temp\app\pequalnp\mathlib-cache'
$env:PATH = "$env:ELAN_HOME\bin;" + $env:PATH
Set-Location C:\Users\temp\app\pequalnp\pequalnp-proofs\lean
lake build PEqualNP.Outside.<ModuleName>
```

## Repo
- Fork clone: `C:\Users\temp\app\pequalnp\pequalnp-proofs` (origin = remiserverbackup-arch fork,
  `http.sslBackend=openssl` is set locally because the SChannel path fails in this sandbox).
- Push credentials: `gh` lives at `C:\Users\temp\app\pequalnp\tools\bin\gh.exe`,
  config dir `C:\Users\temp\app\pequalnp\ghconfig` (set `GH_CONFIG_DIR` to it).
  `gh auth token` works; git push needs
  `-c "http.extraheader=AUTHORIZATION: basic <base64 of x-access-token:TOKEN>"`.
- The working tree must stay clean: every proof is a single NEW file under
  `lean/PEqualNP/Outside/`, nothing else changed.

## Desk client
- `C:\Users\temp\app\pequalnp\pnp.py` — `next / beat / release / step / show`.
  Uses urllib because `curl.exe` cannot do TLS here (SChannel `SEC_E_NO_CREDENTIALS`).
- **We share one handle quota** (8 draws / 10 min). Coordinate before drawing.

## What the DSH agent runs
- `pequalnp\work\grind2.py` — unattended loop: draws steps, keeps `RepSizeAtMost` and small
  `exists C` steps, builds the Lean file, pushes a branch and opens the PR.
- Log: `pequalnp\work\grind2.log`. Do not kill it without telling Boss.

## Suggested split (already agreed in the chat)
- GPT: large-graph steps, lower-bound / certified-search steps.
- DSH agent: small circuit witnesses, n <= 12.
