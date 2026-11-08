# Issue triage (OV-59, 2026-11-08)

The four open GitHub issues were read on 2026-11-08 and compared with the 1.1.0 source. No reply was posted:
replies are an owner action, and a useful one needs a device result (D-03).

| Issue | Report | Assessment against 1.1.0 |
|---|---|---|
| #4 | App closes right after tapping it; README not valid | Most likely one of the four unsupported 2025-07-31 builds (README → Install). In 1.1.0, `MainActivity.onCreate` catches startup failures and shows a fallback screen with the error instead of closing. Not reproduced: no device run |
| #8 | App closes itself; nothing shown | Same as #4 |
| #6 | How to use it after installing the APK | The app alone does nothing: it needs Termux Monet, the CLI `.deb`, the "Run commands in Termux" permission and `allow-external-apps=true` (README → Requirements, Install). The 2025 builds' README predates this |
| #7 | Doesn't work; no usage details | Same as #6 |

**Suggested owner reply, once a 1.1.0 build has passed `docs/DEVICE_RC_CHECKLIST.md` on a device:** the
2025 builds are unsupported; install 1.1.0 and follow README → Requirements and Install. Then close #4–#8.
Until then, F79 stays open.
