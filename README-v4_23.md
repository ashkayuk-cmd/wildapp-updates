# Wild App — v4.23 (APK build 137)

**One file, one install.**

1. Hold BACK for 10 seconds → "Leave".
2. Install `WildApp.apk`. Same key, corrections and history kept.
3. Set WildApp as the default launcher again.
4. Upload to the repo as `WildApp.apk`.

---

## My mistake, fixed

The bar-flash fix in the last build hid two things called `.qrbar` — but that
name is used for **two different bars**: the camera-scanner controls (correct
to hide, unreachable in this build anyway) and the **handheld top bar with
RESET and the gear** (should always be visible). I hid both without checking
which was which, so the second one — the one you actually use — disappeared
along with it.

Only the camera-scanner one is hidden now. RESET and the gear are back, and
the white flash on restart/scroll is still gone — that fix only ever touched
the desktop tab strip, which was the real source of the flash.

Checked in the built file this time, not just assumed: confirmed the RESET
button and its parent bar render with no `display:none` anywhere in the chain,
inside the actual `app.html` sitting inside this APK. Full 61-test suite still
green, render diff still 0 against your last working build.

## Stamps

`versionCode 137` · `THIS_BUILD_NUM=137` · `BUILD_NAME="v4.23"` ·
`BAKED_DATA_BUILD=137` · `REPO_APK_BUILD=137` · data unchanged (a5195d2e,
24,102 rows)

`WildApp.apk` — 1,119,153 bytes
