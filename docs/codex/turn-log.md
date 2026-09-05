# Codex turn log

## 2026-09-05 — Public-readiness location audit and Actions setup

- `scope`: Android manifest and source, macOS source and entitlements, GitHub Actions workflows, and all fetched remote branch tips
- `changed`: `README.md`, `PRIVACY.md`, `docs/codex/turn-log.md`, and Swift call-site compatibility fixes in `MacHost/Sources/AppDelegate.swift`, `MacHost/Sources/ControlPortResolver.swift`, `MacHost/Sources/StreamingServer.swift`, and `MacHost/Tests/SideScreenTests/PairingURLTests.swift`
- `validation`: static location scan found zero location permissions, APIs, filenames, or coordinate data; workflows use `contents: read` and contain no location access or upload step; local `swift test --package-path MacHost` passed 52 tests; the first hosted macOS run exposed Swift 5 parser incompatibility with trailing commas and was fixed
- `evidence`: implemented
- `blocker`: rerun hosted macOS after the compatibility fix; hosted Android and final public-visibility verification remain pending
- `next`: push the compatibility fix, observe both workflows, then make the repository public
- `rollout_refs`: current Codex session
