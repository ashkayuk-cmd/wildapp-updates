WILD APP — v1.78 / build 74   (APK — install this)
==================================================

INSTALL
-------
1. On the PDA, HOLD THE BACK BUTTON for 10 seconds -> tap "Leave".
2. Open WildApp.apk and install. Same signing key, so it goes over the top —
   your corrections, added addresses and scan history all survive.
3. Then upload the same WildApp.apk to
   github.com/ashkayuk-cmd/wildapp-updates, replacing the old one.

Nothing else to upload. This APK already carries v1.77 (the W2 1PN firm
option and the shared-street tags), so the index.html I sent yesterday is
included — if you never uploaded it, it doesn't matter now.


1D BARCODES REGISTER AGAIN
--------------------------
The scanner filter I added in v1.76 is gone, all of it:

* A 1D barcode is handed to the app again whatever it says — postcode or
  no postcode.
* Tracking numbers included, as you asked. They have no address on them,
  so they show "No postcode found", buzz, beep and land in Recent scans
  like any other read — registered, not swallowed.
* The web-side rule that silently ignored tracking numbers is gone too,
  so nothing else is quietly dropping them behind the scenes.

Your scanner beep comes back with it — v1.76 turned off the engine's own
decode beep, and that's undone.


SOUND BACK TO HOW IT WAS
------------------------
Everything I changed in v1.75 is reverted, exactly:

* Sine wave again, not square.
* The original pitches and lengths: exact 1500 + 1900 Hz, building 1400,
  picker 950 twice, no-match 420, the low FIRM tail 640.
* The old volume curve — 0.6 x (level/100) squared. At the default 70%
  that's noticeably quieter than v1.75 was. If it ends up too quiet on the
  round, the Sound screen slider still goes to 100.
* The volume keys no longer set the app's beep level. They move the device
  volume and show the "Volume 12 / 15" popup, as before; the beep level is
  set on the Sound screen only, and no preview tone fires on a key press.

0% on the Sound slider still means no beep, buzz only.


HOW IT WAS BUILT
----------------
I didn't hand-edit the scanner code out — I put the ORIGINAL code file back.
Your repo still had the v1.66 APK on it, and v1.76 was built on exactly that
code, so its classes.dex is byte-for-byte the app before the filter existed.
The old sound values came from the same place, so they're the real ones
rather than my reconstruction.


VERIFIED
--------
versionCode 74 (installed app is 72) · same signing key
(2C:3A:BB:7A:B7…) · signatures v1+v2+v3 · zipaligned.

* Code file identical to the pre-filter one; the three added methods
  (shouldSkipScan, is2D, applyScanConfig) are gone and nothing else moved.
* Every untouched file byte-identical to build 72, same entry order and
  compression; 24,149 addresses.
* 31 tests on the reverts (tracking codes logged and shown, every old tone
  and the old volume curve, volume keys leaving the beep level alone) and
  the 91 v1.77 tests re-run against the app packaged inside this APK.
* 260-label comparison against v1.77: one difference, the tracking-number
  card's wording.

WildApp.apk — 1,102,769 bytes, sha256 47527522…
