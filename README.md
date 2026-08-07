# Wild App — v1.61 (build 57)

Same key as always, so it installs over v1.60 and keeps your corrections and
history. One change only.

---

## Install it

1. Hold **BACK** for 10 seconds → **Leave**.
2. Open the downloaded `WildApp.apk` and install.
3. Launch it — the bottom-left pill should read **v1.61 · build 57**.
4. Upload it to the repo as **`WildApp.apk`** and set **`version.txt`** to **`57`**.
   Leave `content.json` alone.

---

## The flashing nav bar and notification bar

`onResume()` was calling `requestAdmin()` — every single time the app came back
to the foreground. If Android's device-admin permission isn't granted, that
method opens the system **"Activate device administrator?"** screen. The kiosk
then broadcasts close-dialogs and drags itself back to the front within 80 ms,
so the system screen never gets to settle: what you see is its status bar and
nav bar flashing at the top and bottom of the screen.

It was there all along and invisible, because screen pinning blocked the app
from launching other activities. Take the pin away in v1.60 and the launch goes
through.

**The fix:** that call is removed from `onResume`. The app never asks for device
admin again. Nothing else in the resume path changed — the scanner still resumes
and the screen-off timer is still armed.

I also removed the leftover `stopLockTask` call from the same method. It was
there to unpin the screen once after updating from v1.60, which has already
happened on your PDA, so it now runs for no reason.

### If you ever want the 3-minute screen-off

That permission exists for one feature: switching the PDA screen off after 3
minutes idle. It still works — you'd just grant it yourself, once, in
**Settings → Security → Device admin apps → Wild App**, instead of the app
nagging you for it. Until then the app's own black screen at 8 minutes carries
on as it does now.

---

## Checked before delivery

- **Signature**: same certificate (`2C:3A:BB:7A:B7:00:AF:19…`), v1 + v2 + v3,
  zip-aligned.
- **Only four files differ** from v1.60: the code file, the manifest
  (versionCode 56 → 57) and the two app HTML files (version stamps only). The
  addresses, spreadsheet, libraries and icon are byte-for-byte identical —
  24,147 addresses.
- **Nothing calls `requestAdmin` anywhere** in the app any more, and `onResume`
  is the same length as before — the calls were replaced in place, so no other
  code moved.
- **110 tests** re-run against the app as packaged inside this APK: your
  Lancaster Hall barcode, the sleep screen, the everyday scans, and checks that
  the app won't wrongly claim an update afterwards — with `version.txt` at 56
  *or* 57, so it behaves either way while you're mid-upload.

`WildApp.apk` — 1,082,289 bytes, sha256 `2f6be48a81c0751d…`
