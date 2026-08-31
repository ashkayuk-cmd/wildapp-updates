WILD APP — build 197 (v4.82)   APK + OTA
========================================

UPLOAD BOTH to ashkayuk-cmd/wildapp-updates (main):
  WildApp.apk
  index.html
No wild_data.json — addresses unchanged, and the copy baked in this APK is
byte-identical to the one in the APK you sent me (24,102 rows, a5195d2e).

THEN INSTALL WildApp.apk on the PDA. All three of today's items need the new
bridge inside the APK; the OTA file alone will not do them.

After installing, re-select Wild App as the default launcher — installing over
an existing APK always clears that.

Signed with your original key. The certificate in this APK is byte-identical to
the one in the APK you uploaded (SHA256 2C:3A:BB:7A:B7:00:AF:19…), so it goes on
as an update. Corrections and scan history are kept.

versionCode stays 193 on purpose. The app reads its own APK build from the baked
app.html (now 197), not from versionCode, and editing the binary manifest for a
number nothing reads is risk for no gain.


WHERE THE STARTING POINT CAME FROM
----------------------------------
Build 196 was never published — the repo was still on 195 and the APK you sent
was 193, so 196's pull-down work existed nowhere. I rebuilt it into this one.
197 therefore carries everything from 194, 195 and 196 as well as today's three.


1. BATTERY PERCENT IS NOW THE REAL ONE
--------------------------------------
The pill was reading the level/scale pair out of Android's BATTERY_CHANGED
broadcast. On this TC56 that pair is coarse and lags, which is why the number
would sit still for most of a round and then drop several points at once.

New bridge method battery2() reads BATTERY_PROPERTY_CAPACITY straight off the
fuel gauge — the same source Android's own battery screen uses — and falls back
to the old broadcast if the gauge does not answer. The pill now refreshes every
15 seconds instead of 30, since the gauge actually moves in 1% steps.

Charging state still comes from the broadcast, so the bolt and the green fill
behave exactly as before.


2. CLOSE OTHER WINDOWS  (Back button menu → Housekeeping)
---------------------------------------------------------
Finishes every window the app has left open apart from the one in front, then
pulls the scan screen back to the front. It reports what it did — "Closed 2
other windows", or "Nothing else was open".

WHAT IT CANNOT DO, so you are not caught out: Android does not let one app close
another app's windows without device-owner rights, which this app does not have.
So this clears Wild App's own strays — which is what actually accumulates on a
round — and brings the app forward over anything else that is showing. It cannot
force-stop, say, Chrome or StageNow. Exiting those still needs kiosk mode, which
is already the row above.


3. BROWSE FILES  (Back button menu → Housekeeping)
--------------------------------------------------
Opens the PDA's own Files app. On a device without one it falls back to the
system file picker, which browses the same storage.

Useful for reading the crash log at
  Android/data/uk.wild.app/files/wild-crash.txt

IF KIOSK MODE IS ON, Android blocks the app from starting another app and the
row says so in plain words rather than appearing to do nothing. Turn kiosk off
on the row above, browse, then turn it back on.


4. NOTIFICATION PULL-DOWN — the FAILURE fix (carried from 196)
--------------------------------------------------------------
WHAT WAS WRONG: pullDown() in the old APK hard-coded UiMgr version "10.1" into
the MX profile XML. This TC56 is Android 8, whose MX is older than that, so it
rejected the whole profile and handed back the bare word FAILURE.

WHAT I DID NOT DO: hard-code 5.1 instead. That is swapping one guess for
another, and if it were wrong you would need a whole new APK to find out.

INSTEAD: the APK gains pullDownV(mode, version), which takes the version from
JavaScript. The list lives in the web layer, so if this PDA wants something
unusual I can change it over the air without you installing anything.

The app walks a ladder — 5.1, 4.4, 6.0, 7.0, 8.2, 9.2, 10.1 — stops at the first
version the device accepts, and remembers it, so every later change is a single
call. If MX returns a real error rather than a rejection (EMDK not open, no
PROFILE feature) it stops immediately instead of hammering the device six more
times. If everything is refused it says which versions it tried and forgets the
remembered one so the next go starts clean.


STAMPS
------
  THIS_BUILD_NUM   197
  BUILD_NAME       v4.82
  REPO_APK_BUILD   197
  BAKED_DATA_BUILD 193        (unchanged)
  REPO_DATA_BUILD  131        (unchanged)
  REPO_DATA_HASH   a5195d2e   (unchanged, verified against baked data)
  REPO_DATA_ROWS   24102      (unchanged, verified against baked data)


TESTED
------
  - dex round-trip control diff: the ONLY class that changed is KioskBridge,
    and within it nothing was removed — four methods added, seven untouched
  - 15 tests on the pull-down ladder: finds the working version and stops,
    reuses it next time, walks past failures, tries a stale remembered version
    once and not twice, aborts on a real error, reports every version when all
    are refused, and falls back safely on an old APK with no pullDownV
  - 11 tests on the battery pill: gauge preferred, charging and low-battery
    colours, a dud gauge reading falls through to the old bridge rather than
    hiding the pill, a throwing battery2 is caught, old wrappers unchanged,
    out-of-range values clamped
  - 19 tests on the two new rows: absent on old wrappers and on the website,
    correct IDs, singular/plural wording, errors surfaced rather than swallowed,
    and no use of the .bhRow class the existing pick() wiring owns
  - full app booted in jsdom with the real dataset: no page errors, the five
    original back-menu rows all still present alongside the two new ones, and
    the pull-down screen still renders
  - all 6 script blocks parse
  - APK re-opened after signing: parses, zip integrity OK, dex SHA1 and Adler32
    both correct, no original entry lost or added, the nine untouched entries
    byte-identical to yours, and the OTA index.html byte-identical to the
    app.html baked inside


ON THE HORIZON (unchanged)
--------------------------
  - wild_data.xlsx master is still behind the shipped JSON
  - BUCKHILL LODGE: A-Z sheet says Bayswater Road, address data says Hyde Park
  - KENDAL STREET street assignment and PARK WEST slash value still awaiting
    your confirmation
