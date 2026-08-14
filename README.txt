WildApp v2.0 — APK build 95
===========================

WHAT'S NEW
  * The recents (square) key can no longer take the app off the screen.
    The app pins itself to the screen every time it starts.
  * Everything from the content builds 91-94 is baked in, so this APK is
    fully up to date on its own:
      - PIN on the way in to App version (once, not on every button)
      - Website version bar at the top of the main screen (tap to copy)
      - Recent scans moved off the result screen into Settings
      - the plug icon gone from the Recent list
      - Back works on a result screen (Recent list / postcode keypad / idle)
      - postcode keypad: 3 characters, no space key
      - the "Scan a barcode" home text no longer disappears after Back

INSTALLING
  Hold BACK for 10 seconds, tap "Leave", then install this file.
  (The kiosk fights the installer otherwise.)
  Same signing key as before, so it installs OVER the top: your
  corrections, added addresses and scan history are all kept.

FIRST LAUNCH AFTER INSTALLING
  Android will ask once to confirm screen pinning - tap "Got it".
  If you tap the wrong thing, no harm done: it asks again the next time
  the app comes to the front.

GETTING OUT WHEN YOU NEED TO
  The 10-second BACK hold still works exactly as before - it unpins the
  app first, so "Leave" behaves the same as it always has.
  Android's own way out is holding BACK and RECENTS together.

UPLOADING
  Upload this file to the repo as WildApp.apk (exactly that name).
  You do NOT need to upload index.html - build 95 is baked into this APK
  and is newer than anything in the repo.

BUILD DETAIL
  versionCode 95, THIS_BUILD_NUM 95, BAKED_DATA_BUILD 95, REPO_APK_BUILD 95.
  Address data unchanged (24,149 rows, data build 72).
  Native change is confined to MainActivity: maybePin() now calls
  startLockTask() (it had been left calling stopLockTask() since v1.60) and
  onResume() calls it, so pinning re-arms if Android ever drops it. No
  device-admin request is made - that was the thing that flashed the bars
  in v1.60 and it stays removed. Method set is identical to build 86;
  MainActivity is the only class whose body changed; all 8 other entries
  are byte-identical to the published APK.
