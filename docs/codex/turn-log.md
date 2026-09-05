# Codex turn log

## 2026-09-05 — Public-readiness location audit and Actions setup

- `scope`: Android manifest and source, macOS source and entitlements, GitHub Actions workflows, and all fetched remote branch tips
- `changed`: `README.md`, `PRIVACY.md`, `docs/codex/turn-log.md`, and Swift call-site compatibility fixes in `MacHost/Sources/AppDelegate.swift`, `MacHost/Sources/ControlPortResolver.swift`, `MacHost/Sources/StreamingServer.swift`, and `MacHost/Tests/SideScreenTests/PairingURLTests.swift`
- `validation`: static location scan found zero location permissions, APIs, filenames, or coordinate data; workflows use hosted `ubuntu-latest`/`macos-14`, `contents: read`, and no location access or upload step; local `swift test --package-path MacHost` passed 52 tests; hosted Android run `33989543211` and hosted macOS run `33989543209` passed; repository Actions runner inventory was empty (`total_count: 0`)
- `evidence`: implemented, tested, and live; final visibility is public, anonymous GitHub API access returned HTTP 200, `PRIVACY.md` returned HTTP 200, and the repository runner inventory remains empty (`total_count: 0`)
- `blocker`: none for the requested publication gate; this is not a legal opinion and third-party dependency terms remain their respective responsibility
- `next`: keep future workflows on hosted runners and rerun the location audit before materially expanding telemetry or permissions
- `rollout_refs`: current Codex session
