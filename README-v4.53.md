# Wild App v4.53 — APK build 168

**Diagnostic build.** No feature changes, no change to how the app scans,
resolves or renders. It only writes more lines to the crash log so the next
time the scanner wedges we can see what actually happened.

## Install
Hold BACK for 10 s → "Leave", then install over the top. Same signing key, so
corrections, added addresses and scan history all survive.

After installing, re-pick Wild App as the home app (any install clears that).

## What's new

Extra lines in `Android/data/uk.wild.app/files/wild-crash.txt`:

- `LIFECYCLE onPause` / `onResume` / `onStop` — every time the app goes to the
  background or comes back.
- `SCANNER STATUS <state>` — every status the EMDK scanner reports. This is the
  important one. The old code silently ignored every status except IDLE, so a
  scanner sitting in ERROR or WAITING never re-armed a read and nothing recorded
  it. That matches your symptom exactly: trigger pressed, no aim light, no beep.
- `SCANNER EMDK opened` / `SCANNER EMDK CLOSED (connection lost)`.
- `RESTART APP called by the app itself` — written at the top of restartApp().

## How to read it next time it happens

Note roughly when the scanner died, then send me the file.

- Last line before the gap is `SCANNER STATUS ERROR` (or WAITING, or anything
  that isn't IDLE) → the scanner wedged in a state the code never re-arms from.
  That's a real fix, and a small one.
- `SCANNER EMDK CLOSED` → something else on the device took the scan engine.
- Lifecycle lines then nothing, and later an `APP STARTED` with no
  `RESTART APP called by the app itself` above it → Android killed the process.
  Different fault, different fix.
- `RESTART APP called by the app itself` before an `APP STARTED` → the app
  restarted itself, and we can then work out which caller did it.

## Content

Bakes repo content build 167 (v4.52) restamped as 168, so this APK is ahead of
everything published. **No index.html upload needed.** Data unchanged
(build 131, hash a5195d2e, 24,102 rows).

## Verified

- versionCode 168, platformBuildVersionCode left at 24.
- 155 methods before and after — none added, none removed.
- Only MainActivity and ZebraScanner bodies changed; the smali diff is exactly
  the note() calls above plus label renumbering.
- 7 other APK entries byte-identical, entry order and compression preserved.
- Signed v1/v2/v3 with CN=Wild App (SHA-256 2C:3A:BB:7A…).
