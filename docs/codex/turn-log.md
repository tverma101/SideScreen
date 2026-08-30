# Codex turn log

## 2026-08-30 — complete PR #43 appliance lifecycle

- Goal: finish the runtime implementation behind PR #43's Mac lock/sleep → Android suspend → wake/reconnect contract.
- Scope: current `tverma101/SideScreen` PR head `6d92f17`; isolated branch `codex/complete-pr-43`; unrelated source checkout remained clean.
- Changed: Mac lifecycle observation and overlap handling; ScreenCaptureKit/VideoToolbox/virtual-display quiesce and wake rebuild; server frame gate, Bonjour advertisement, capability-gated `HostSuspending`, and network-queue lifecycle fencing; Android lifecycle teardown, immediate generation invalidation, screen-awake/decoder/renderer/brightness release, NSD discovery fallback, retry backoff, and explicit-disconnect suppression; README and lifecycle docs; focused tests.
- Validation: `swift test` passed with 66 tests at 2026-08-30 11:25; `ANDROID_HOME=/Users/tejas/Library/Android/sdk ANDROID_SDK_ROOT=/Users/tejas/Library/Android/sdk ./gradlew test assembleDebug` passed with 36 debug unit tests and the debug APK assembled. The current APK installed successfully with `adb install -r -d` on the connected SM-X800 at 11:25 and `MainActivity` cold-launched successfully; the process log had no Android runtime exception.
- Evidence state: implemented, locally tested, installed, and launch-smoke tested; paired live streaming, lifecycle/hardware cycles, and user confirmation are not established in this turn.
- Blocker/gap: target Mac lock/unlock, system sleep/wake, Android screen-off/on, Wi-Fi return, process restart, and long-cycle hardware receipts remain required before removing Draft status.
- Delivery: commit `8eae642f0756aa4d7aaa8163aed66f55f7b6ec01` was fast-forward pushed to `design/appliance-lifecycle`; PR #43 remains open, Draft, and mergeable/clean. No hosted Actions were run.
- Next action: collect the target-hardware lifecycle receipts and update PR #43's evidence; no further code action is required in this turn.

## 2026-08-30 — PR #43 USB connectivity and display follow-up

- Goal: repair the reported USB Connect crash/black session and preserve strict USB-versus-wireless transport selection.
- Scope: isolated worktree `/Users/tejas/Projects/SideScreen-pr43-completion`, branch `codex/complete-pr-43`; original `eval/runtime-snapshot-2026-08-23` checkout remained untouched.
- Changed: Android teardown no longer draws into or backgrounds a `TextureView`/`SurfaceView` during cleanup; decoder failure and first-render timeout invalidate the active generation; SGSR/CfL viewports follow fullscreen surface resizing; USB validates loopback and Mac listeners enforce selected transport; wireless discovery avoids stale cached LAN endpoints; invalid QR and live USB↔wireless mode changes now have explicit repair/guard paths; focused tests, troubleshooting notes, and the control/resource-impact map added.
- Validation: Mac `swift test` passed 68 tests at 12:20; Android `./gradlew test assembleDebug` passed with a successful debug build at 12:19; installed APK and canonical Mac app completed two USB connect cycles, including Mac stop → Android EOF/teardown → Mac restart → fresh reconnect. Both cycles kept `MainActivity` alive, decoded a first frame, and rendered the full `2800×1752` panel without the prior black strip.
- Evidence state: implemented, locally tested, installed, and USB live-smoke tested. Wireless transport admission is source-tested and the stale-address failure was reproduced; a fresh physical wireless stream cycle and long lifecycle receipts remain unverified. User confirmation is still pending.
- Blocker/gap: Mac lock/sleep, Android screen-off/on, Wi-Fi return, process restart, and long-cycle hardware receipts remain required before removing Draft status.
- Delivery: implementation commit `49995b201bc3e1265be8600204a2490c55521be3` was fast-forward pushed to `design/appliance-lifecycle`; remote PR head matches. PR #43 remains open, Draft, and mergeable; no hosted Actions or PR-state mutation was performed.
- Next action: user acceptance on the physical wireless route plus Mac lock/sleep and Android screen-off/on cycles; these are the remaining evidence gates, not uncommitted code work.
