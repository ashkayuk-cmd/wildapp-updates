WildApp v4.78 — APK build 193
=============================
Supersedes build 192, which was never installed. Everything from 192 is
in here.

INSTALL
  Hold BACK for 10 seconds, tap "Leave", then install.
  Afterwards: re-pick Wild App as the home app (any install clears it),
  and hold BACK -> "Turn on" to put kiosk mode back.
  No index.html upload needed — build 193 content is baked in.
  versionCode 193, signed v1/v2/v3 with wild-signing.keystore.


WHAT'S NEW vs 192

  The notification pull-down control has MOVED off the Options menu and
  onto the App version screen, on its own row under "Update app from
  GitHub". It shows the current state on the button itself —
  "Notification pull-down: Blocked / Allowed / not set" — and tapping it
  opens the same two-tap screen as before, whose Back now returns to App
  version rather than Options.

  The Options menu is back to the row set it had before 192.

  No PIN was added: openAppVersion already asks for 1984 at the door
  (v1.96), so the whole screen including this button is already behind it.


CARRIED FROM 192

  Notification pull-down toggle. The shade is an Android setting, so the
  web layer can't touch it — the app submits a UiMgr/NotificationPullDown
  MX profile through EMDK, reusing the EMDK connection the scanner
  already holds. Value 1 = allowed, 2 = blocked, the same parameter as in
  your StageNow profile.

  The screen prints whatever MX replies and only saves on success. If it
  reports anything else the device is refusing to let the app submit MX
  settings; AccessMgr's AllowSubmitXMLPackageNames with uk.wild.app on it
  is the likely unlock. Every attempt is logged to wild-crash.txt as
  "PULLDOWN mode=N -> ...".

  Your choice is re-applied ~4 s after boot, since StageNow or an
  Enterprise Reset can move it underneath the app. Until you choose, the
  app leaves the device alone.

CARRIED FROM 191

  The two idle buttons (Type a postcode / Type a Street or Building name)
  are #074150, the same fixed size (280px, stacked), raised, and sink
  when tapped.


VERIFICATION

  classes.dex BYTE-IDENTICAL to build 192 — this was a content-only
  change, no native edit. 8 of 11 entries byte-identical; only app.html,
  index.html and AndroidManifest.xml differ.

  Native layer (added in 192, unchanged here): 2 methods added, 0
  removed, 0 existing bodies changed —
    ZebraScanner.applyMx(String)   submits MX XML via ProfileManager
    KioskBridge.pullDown(int)      the JS bridge, @JavascriptInterface
  plus static field ZebraScanner.emdkRef set in onOpened. A full baksmali
  round-trip showed ZebraScanner.smali and KioskBridge.smali as the only
  files differing, and neither new method writes into its own parameter
  registers.

  t193.cjs 13/13 on the app.html inside the signed APK. The same suite on
  build 192 fails exactly the relocation cases then throws reaching for a
  button that isn't there yet — the change reproducing.
  250-label render diff vs 192 = 0.
  t192.cjs is superseded: its one failure on this build is "menu row
  appears", which is the removal you asked for.


STILL UNVERIFIED — I have no device
  Whether EMDK grants a sideloaded app the PROFILE feature on your MX
  10.3. If not, the toggle reports the error rather than failing
  silently. Send the wild-crash.txt line and I'll know which way it went.
