WildApp v4.84 — build 199
=========================
Signed with wild-signing.keystore (CN "Wild App", 2C:3A:BB:7A:B7:00:AF:19…),
v1/v2/v3, zipaligned. Installs OVER the existing app — corrections, added
addresses and scan history are kept.


WHAT'S NEW: RELEASE THE SCANNER
-------------------------------
Wild App binds the hardware trigger through EMDK, and that binding is
exclusive. While it holds the scan engine no other app on the PDA can scan —
StageNow included. That is why a staging barcode reads on one device and does
nothing on another.

You can now hand the engine over without uninstalling.

  ⚙️ Settings → Back button menu → Scanner → "Release the scanner"

Asks for the PIN (1984), then confirms. Once released:

  • StageNow and DataWedge can scan normally.
  • Wild App does NOT read barcodes until you give it back.
  • The card changes to "Scanner released" with a button to hand it back.

To give it back, either tap "Give the scanner back to Wild App" on that same
card, or just restart the app — the release is deliberately NOT saved, so a
full restart always comes back scanning. You cannot strand yourself.

Nothing on the PDA is lost. This is the alternative to uninstalling, which
wipes that device's corrections, added addresses and scan history.


TO INSTALL
----------
Hold BACK 10 seconds → "Leave", then install. The kiosk fights the installer
otherwise. Re-pick Wild App as the Home app afterwards — any install clears
the preferred-launcher setting by itself.


NOTHING TO UPLOAD
-----------------
The v4.84 content is baked into this APK (app.html restamped to build 199,
loader BAKED 199). No index.html or wild_data.json upload is needed.


IF STAGENOW STILL WON'T SCAN AFTER THIS
---------------------------------------
Then the exclusive binding was not the cause on that PDA, and the fault is on
the device rather than in the app. Check, comparing against a PDA where
StageNow does work:

  • DataWedge → active profile → Barcode input is enabled, and no profile
    lists com.symbol.stagenow with scanning off.
  • An MX scanner-disable policy left by an earlier staging barcode.
  • Multi-part staging barcodes — every part has to be read in sequence; a
    partial read looks like nothing happening at all.


VERIFICATION
------------
versionCode 199 (platformBuildVersionCode left at 24).
dex diff vs the installed build: 5 methods added
  KioskBridge.scanRelease / scanResume / scanHeld
  MainActivity.scanHold / scanUnhold
3 bodies changed, and only these
  KioskBridge.run (dispatch for the two new idx values)
  ZebraScanner.resume, ZebraScanner.start (skip re-acquiring while released)
0 methods removed. 7 untouched zip entries byte-identical, original entry
order and compression preserved (resources.arsc and ic_launcher.png stay
stored as-is). t84.cjs 10/10 — the card in both states, and with no wrapper
bridge present it renders empty instead of throwing.


ONE THING I NOTICED
-------------------
The APK you uploaded had versionCode 193 while its baked content said build
198 / REPO_APK_BUILD 198. Those should match. It means an OTA content update
landed on an APK that was never itself rebuilt, so the update checker was
reporting an APK build the device did not actually have. This build sets all
of them to 199, which clears it.
