WildApp v4.77 — APK build 192
=============================

INSTALL
  Hold BACK for 10 seconds, tap "Leave", then install. The kiosk fights
  the installer otherwise.
  After installing, re-pick Wild App as the home app (any install clears
  it), and hold BACK -> "Turn on" to put kiosk mode back.

  No index.html upload is needed — build 192 content is baked into this APK.
  versionCode 192, signed v1/v2/v3 with wild-signing.keystore.


WHAT'S NEW

1. Notification pull-down toggle  (the thing you asked for)
   Settings -> "Notification pull-down". Shows the current state and a
   two-tap button to change it. Behind the gear PIN you already have.

   How it works: the shade is an Android setting, so the web layer can't
   touch it. The app now submits a UiMgr/NotificationPullDown MX profile
   through EMDK, reusing the EMDK connection the scanner already holds.
   Value 1 = allowed, 2 = blocked — the same parameter as in your
   StageNow profile.

   The screen prints whatever MX replies. If it says anything other than
   SUCCESS, the device is refusing to let the app submit MX settings, and
   nothing is saved. In that case the StageNow profile is the way to
   change it, and AccessMgr's AllowSubmitXMLPackageNames may need
   uk.wild.app added before the in-app toggle can work at all.

   Your choice is saved and re-applied ~4 seconds after boot, since a
   StageNow run or an Enterprise Reset can move the setting underneath
   the app. Until you make a choice, the app leaves the device alone.

   Every attempt is written to wild-crash.txt as "PULLDOWN mode=N -> ...".

2. Build 191 content (carried, you hadn't uploaded it)
   The two idle buttons — Type a postcode / Type a Street or Building
   name — are now #074150, the same fixed size (280px wide, stacked), and
   raised so they read as buttons, sinking when tapped.


NATIVE CHANGES
  2 methods added, 0 removed, 0 existing bodies changed:
    ZebraScanner.applyMx(String)  — submits MX XML via ProfileManager
    KioskBridge.pullDown(int)     — the JS bridge, @JavascriptInterface
  Plus a static field ZebraScanner.emdkRef, set in onOpened, so applyMx
  reuses the scanner's own EMDK handle rather than opening a second one.
  A full baksmali round-trip shows ZebraScanner.smali and
  KioskBridge.smali as the only files differing. Neither new method
  writes into its own parameter registers.


TESTS
  t192.cjs 20/20 (bridge absent -> no menu row; two-tap arming; a failed
    MX result does not persist; boot re-apply only after a choice)
  t191.cjs 16/17 on the file inside the APK — the one failure is its own
    build-number assert, which now reads 192
  250-label render diff vs build 191 = 0
  7 of 11 entries byte-identical; only classes.dex, app.html, index.html
    and AndroidManifest.xml changed


UNVERIFIED — I have no device
  Whether EMDK grants a sideloaded app the PROFILE feature on your MX
  10.3. If it doesn't, the toggle will report the error rather than fail
  silently. Send me the line from wild-crash.txt and I'll know which way
  it went.
