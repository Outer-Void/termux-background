# Release checks

The standalone release gate: everything below passes on the release commit, plus at least one device in
[DEVICE_RC_CHECKLIST.md](DEVICE_RC_CHECKLIST.md). Commands are from the repository root.

| Check | Command | 2026-10-27 (Stage 32, local, PRoot Debian 13 aarch64) |
|---|---|---|
| Shell lint | `shellcheck -x termux-background-deb/data/data/com.termux/files/usr/bin/termux-background build-deb.sh tests/*.sh` | Pass (clean) |
| CLI regression suite (legacy and Monet backends, bridge protocol, failure exit codes) | `./tests/cli_test.sh` | Pass |
| Debian package build | `./build-deb.sh` | Pass (`termux-background_1.1.0_all.deb`) |
| Package metadata, payload, executable, version | `./tests/package_test.sh` | Pass |
| Android unit tests | `./gradlew test` | Pass: 10 tests in each of debug and release |
| Android lint | `./gradlew lint` | Pass: 0 errors, 14 warnings (incl. `OldTargetApi`: target 35, 36 available) |
| APKs | `./gradlew assembleDebug assembleRelease` | Pass. Release `com.termuxbackground` 1.1.0, min SDK 24, target 35, only permission `com.termux.permission.RUN_COMMAND` (plus the app's own receiver permission); unsigned (no keystore here) |
| CI (`.github/workflows/build.yml`) | push to `main` | **Blocked:** no Outer-Void Actions job starts (account billing lock) |
| Device checklist | [DEVICE_RC_CHECKLIST.md](DEVICE_RC_CHECKLIST.md) | **Not run:** no device has passed it |

On aarch64 hosts, Gradle needs a native AAPT2: add `-Pandroid.aapt2FromMavenOverride=/path/to/aapt2`
(this run used Termux's `/data/data/com.termux/files/usr/bin/aapt2`) and set `ANDROID_HOME` to the SDK.

Not a release yet: a release needs a signing key in CI secrets, a tag `vX.Y.Z` (CI then signs, verifies with
`apksigner` and publishes), and a passed device checklist.
