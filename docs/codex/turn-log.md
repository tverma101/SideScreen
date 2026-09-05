# Codex turn log

## 2026-09-05 — Public-readiness location audit and Actions setup

- `scope`: Android manifest and source, macOS source and entitlements, GitHub Actions workflows, and all fetched remote branch tips
- `changed`: `README.md`, `PRIVACY.md`, `docs/codex/turn-log.md`
- `validation`: static location scan found zero location permissions, APIs, filenames, or coordinate data; workflows use `contents: read` and contain no location access or upload step; hosted runs pending at commit time
- `evidence`: implemented
- `blocker`: hosted Android/macOS runs and final public-visibility verification remain pending
- `next`: enable Actions, fast-forward `main` to the verified source, observe both workflows, then make the repository public
- `rollout_refs`: current Codex session
