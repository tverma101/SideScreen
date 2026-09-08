# Codex turn log

## 2026-09-05 — Public-readiness location audit and Actions setup

- `scope`: Android manifest and source, macOS source and entitlements, GitHub Actions workflows, and all fetched remote branch tips
- `changed`: `README.md`, `PRIVACY.md`, `docs/codex/turn-log.md`, and Swift call-site compatibility fixes in `MacHost/Sources/AppDelegate.swift`, `MacHost/Sources/ControlPortResolver.swift`, `MacHost/Sources/StreamingServer.swift`, and `MacHost/Tests/SideScreenTests/PairingURLTests.swift`
- `validation`: static location scan found zero location permissions, APIs, filenames, or coordinate data; workflows use hosted `ubuntu-latest`/`macos-14`, `contents: read`, and no location access or upload step; local `swift test --package-path MacHost` passed 52 tests; hosted Android run `33989543211` and hosted macOS run `33989543209` passed; repository Actions runner inventory was empty (`total_count: 0`)
- `evidence`: implemented, tested, and live; final visibility is public, anonymous GitHub API access returned HTTP 200, `PRIVACY.md` returned HTTP 200, and the repository runner inventory remains empty (`total_count: 0`)
- `blocker`: none for the requested publication gate; this is not a legal opinion and third-party dependency terms remain their respective responsibility
- `next`: keep future workflows on hosted runners and rerun the location audit before materially expanding telemetry or permissions
- `rollout_refs`: current Codex session

## 2026-09-07 — Wireless 60 FPS transport and native Android path

- `scope`: local Mac wireless sender, Android wireless receiver, reconnect state, native presentation path, and public-readiness audit
- `changed`: added shared wireless freshness/session policies; capped wireless capture/encode sessions at 60 FPS and 40 Mbps average/60 Mbps one-second peak; added byte-aware, generation-fenced sender pressure; enlarged the wireless Android socket/input buffers; added native SurfaceView decoder targeting, stale-output release, and policy tests; updated `README.md`, made `docs/wireless-60fps.md` explicitly trackable, and ignored local APK recovery backups in `.gitignore`
- `files`: `MacHost/Sources/AppDelegate.swift`, `ScreenCapture.swift`, `StreamingServer.swift`, `VideoEncoder.swift`, `WirelessFreshnessPolicy.swift`, `WirelessSessionProfile.swift`, `WirelessTransportPressure.swift`, their wireless tests, `AndroidClient/app/src/main/java/com/sidescreen/app/MainActivity.kt`, `StreamClient.kt`, `VideoDecoder.kt`, the Android wireless policy/profile, and the Android policy test
- `validation`: `swift test --package-path MacHost` passed 57 tests; `(cd AndroidClient && ./gradlew testDebugUnitTest --no-daemon)` passed 27 tasks/tests; `git diff --check` passed; `./scripts/build_mac.sh` produced a valid signed universal `SideScreen.app` and DMG; `./scripts/build_android.sh` and `./scripts/install_android.sh` produced and installed the debug APK, with recovery artifacts under `backups/apk/20260907T232317Z`; the rebuilt artifacts completed a live wireless handshake on SM-X800, native `c2.qti.hevc.decoder` decode at a 60 Hz target, SurfaceView presentation, touch control, zero decoder drops in sampled windows, and approximately 11–12 ms average decode latency; a bounded animated virtual-display probe rendered correctly but the host's observed changed-frame rate remained approximately 18–24 FPS
- `public_readiness`: repository landing page returned HTTP 200; tracked-file audit found no high-confidence private-key/provider-token markers or sensitive certificate/key filenames; public-facing README, privacy, contribution, license, issue-template, and workflow files are present; no remote branch, PR, merge, workflow, or Actions state was changed
- `evidence`: implemented, tested, packaged, installed, live handshake/native decode, and public HTTP reachability are proven; user-confirmed acceptance is not recorded; the configured 60 FPS ceiling and decoder target are proven, but sustained 60 FPS under a moving real application remains unproven because this host's virtual-display capture probe delivered fewer changed frames
- `blocker`: no implementation/test blocker; remaining performance evidence requires a moving workload that the host virtual-display path actually emits at 60 FPS, plus a longer two-device measurement of sender/receiver FPS, bitrate, stale drops, reconnect, and thermals
- `cleanup`: stopped the rebuilt host, stopped the Android test package, restored the pre-test Mac USB connection preference, moved temporary screenshots/probe logs to Trash, and retained the APK recovery backup; no listeners remain on ports 54321/54322
- `next`: run the same artifact on a representative moving desktop/video workload and capture a sustained 60-FPS trace before claiming full 60-FPS user acceptance
- `rollout_refs`: current Codex session

## 2026-09-07 — macOS duplicate app cleanup and single-install lifecycle

- `scope`: macOS SideScreen app bundles in the user install directory, build/run entrypoints, and the developer install path
- `changed`: added `scripts/cleanup_old_app_copies.sh` with dry-run/apply modes and bundle-ID validation; added `scripts/install_mac.sh` to replace the exact user-facing bundle without creating backups; wired cleanup into `scripts/build_mac.sh` and `scripts/run.sh`; added the canonical `script/build_and_run.sh` and `.codex/environments/environment.toml`; documented the single-copy behavior in `README.md`
- `validation`: pre-cleanup inventory found 45 `~/Applications/SideScreen.app.previous.<timestamp>` bundles, all `com.sidescreen.app` version 0.11.1 and none running; the helper dry-run selected exactly those 45; `--apply` moved all 45 to macOS Trash; post-cleanup `~/Applications` contains only `SideScreen.app`; the new installer replaced that target twice with signed version 0.11.2 and created zero new snapshots; `script/build_and_run.sh --verify` built the universal app, signed it, launched it as a bundle, and verified the process; a controlled snapshot was removed automatically by the normal `scripts/run.sh` path; `bash -n`, ShellCheck, and `git diff --check` passed
- `evidence`: implemented, tested, installed, and locally live launch verification are proven; the old copies are recoverable in Trash; user-confirmed visual acceptance is not recorded; source and app bundles in separate historical workspaces were not removed because they are outside the bounded user install cleanup
- `blocker`: none for the requested macOS install-directory cleanup; Trash was intentionally not emptied
- `cleanup`: stopped the temporary verification launch; no SideScreen process or listeners remain; preserved the current user install and repository build artifacts; no unrelated apps or source worktrees were changed
- `git`: local topic branch only, no push, merge, PR, workflow, or Actions mutation
- `next`: use `./scripts/install_mac.sh --launch` or `./script/build_and_run.sh`; do not invoke the historical installer in an old worktree, since only the canonical repository path now performs backup-free replacement
- `rollout_refs`: current Codex session

## 2026-09-07 — Wired SDR color-range alignment

- `scope`: macOS wired ScreenCaptureKit/CGDisplayStream SDR capture, Android hardware-decoder range signaling, and the installed USB path
- `changed`: made 8-bit video-range `420v` the normal capture format; retained legacy `SideScreen_exp_pixelFormat=8bit` as an explicit full-range A/B control; made the CGDisplayStream fallback follow the selected 8-bit range; taught `PatternInjector` to encode luma/chroma in the destination range; added `VideoColorProfile` tests; updated `README.md` and `CHANGELOG.md`
- `files`: `MacHost/Sources/VideoColorProfile.swift`, `MacHost/Sources/ScreenCapture.swift`, `MacHost/Sources/PatternInjector.swift`, `MacHost/Tests/SideScreenTests/VideoColorProfileTests.swift`, `README.md`, `CHANGELOG.md`
- `validation`: local Swift suite passed 61 tests with 0 failures; `git diff --check` passed; `./scripts/build_mac.sh` produced the signed universal app and DMG; `./scripts/install_mac.sh --launch` installed version 0.11.2 with valid code signing and created 0 app snapshots; the installed sender logged `Stream color profile: 8-bit video`; the connected SM-X800 reported Android decoder `color-range=2 (limited)`, zero decode drops, and repeated frame milestones at the expected 60 Hz receiver cadence with approximately 9–12 ms decode latency
- `evidence`: implemented, tested, packaged, installed, and live wired range/decoder behavior are proven; visual user-confirmed color acceptance is not recorded; sustained wireless 60 FPS remains separately bounded by the prior live evidence
- `blocker`: none for the range implementation; user visual confirmation is still the final acceptance boundary for the reported color symptom
- `cleanup`: temporary auto-start was restored/deleted after validation, and the connected Mac/Android test session plus ADB reverse mappings will be stopped/removed before handoff; the single installed app remains in `~/Applications`, and prior duplicate bundles remain recoverable in Trash
- `git`: local topic branch only, no push, merge, PR, workflow, or Actions mutation
- `next`: visually recheck a representative wired desktop with the installed build; if colors remain wrong, capture paired PixelCopy measurements with default `420v` and explicit `420f` to distinguish a remaining display profile issue from range expansion
- `learning_checkpoint`: `promoted`: source contract plus same-device A/B evidence and current live logs support the limited-range wired SDR fix; `quarantined`: none; `deprecated`: full-range as the normal SDR default; `skipped`: global memory update
- `rollout_refs`: current Codex session

## 2026-09-07 — Topic branch publication

- `scope`: Git publication of the SideScreen Android and macOS work from the canonical checkout
- `changed`: pushed commits `edb14aa`, `66ede46`, and `d468d1a` from `codex/wireless-60fps-native` to `origin/codex/wireless-60fps-native`; no default branch, PR, merge, workflow, or Actions state changed
- `validation`: clean worktree before push; remote accepted the new branch; the remote branch points at `d468d1a`; the published range contains both `AndroidClient` and `MacHost` paths
- `evidence`: source changes, tests, packaging, installation, and live evidence remain documented in the preceding entries; branch publication is proven; user-confirmed acceptance is not recorded
- `blocker`: none for the requested topic-branch push
- `next`: review or open a PR from the published topic branch if desired; default-branch integration remains separately authorized work
- `learning_checkpoint`: `promoted`: scoped topic-branch publication after a clean audited baseline; `quarantined`: none; `skipped`: default-branch integration and Actions
- `rollout_refs`: current Codex session
