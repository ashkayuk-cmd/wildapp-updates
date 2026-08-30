WILD APP — v4.55, APK build 170
================================

WHAT THIS FIXES
The scanner dying after you leave the app (notification shade → gear → HOME),
and "Reset scanner" not bringing it back.

Cause: when the app was backgrounded, the scanner connection could be dropped
altogether (EMDK closes it, or another app takes the scan engine). The app's
resume() only re-enabled a scanner it still had a handle on — if the handle was
gone, it did NOTHING and returned silently. No error, no retry, dead trigger.
"Reset scanner" retried on the same broken connection and gave up after 6 tries
with no further attempt. Only RESET APP worked, because that rebuilt everything
from scratch.

What changed (native, ZebraScanner):
  1. resume() now recovers. If the scanner handle is gone, or enable/read
     throws, it does a FULL re-bind — releases EMDK completely and binds again
     from the start, exactly what a full app restart does. Only kicks in once
     the scanner has worked at least once, so first launch is untouched.
  2. The "gave up" path now escalates instead of stopping. When the 6 quick
     retries fail, it re-binds EMDK from scratch rather than printing
     "init failed" and quitting.
  3. Capped at 3 re-binds so it can't loop; the counter resets the moment the
     scanner comes back, and tapping RESET (top-right) clears it too.
  4. Messages on screen: "Scanner lost — re-binding…" while it recovers, and
     "Scanner could not be re-acquired. Tap RESET (top-right)." only if all
     three attempts fail.
  5. Every re-bind is written to the crash log (Android/data/uk.wild.app/files/
     wild-crash.txt) as "SCANNER full re-bind", so if it ever still fails we
     can see exactly what happened.

Also in this APK: the app content is refreshed to repo build 169 (v4.54),
restamped as build 170 — so nothing you've had shipped is lost, and no
index.html upload is needed for this.

STAMPS
  versionCode 170  (platformBuildVersionCode still 24)
  THIS_BUILD_NUM 170, BUILD_NAME "v4.55"
  BAKED_DATA_BUILD 170, REPO_APK_BUILD 170, loader BAKED 170
  data a5195d2e, 24,102 rows (unchanged)

CHECKS DONE
  - dex diff vs build 168: 1 added method (fullRebind), only ZebraScanner
    changed; every other class byte-identical after a full round-trip.
  - No method writes into its own parameter registers.
  - 7 of 11 APK entries carried through byte-identical; entry order and
    compression preserved; signed v1/v2/v3 with wild-signing.keystore
    (SHA-256 2C:3A:BB:7A…), so it installs OVER the top — corrections, added
    addresses and scan history are kept.
  - The app.html actually inside the signed APK was booted and scanned
    (Sussex Gardens W2 1UL → 14 SUSSEX GARDENS).

INSTALLING
  Hold BACK for 10 s → "Leave", THEN install — the kiosk fights the installer.
  After installing, re-pick Wild App as the home app if Android asks.
  Then hold BACK → "Turn on" to put the kiosk back on.

IF IT STILL DROPS
Send me wild-crash.txt from Android/data/uk.wild.app/files/ — the SCANNER
STATUS / EMDK opened / EMDK CLOSED / full re-bind lines will say whether the
connection is being closed on you or something else is holding the engine.
