Wild App — v4.42, APK build 157
================================

WHAT CHANGED (native)

The BACK-hold menu is now a list instead of three buttons, and it stays
open when you toggle kiosk mode.

  Kiosk mode is ON / OFF          <- title tells you the current state
    Leave kiosk mode  (or  Turn kiosk mode on)
    Exit app
    Launch StageNow
    Android Settings
    Change launcher (Home app)
    [ Close ]

- Tapping the kiosk row toggles it and REOPENS the menu, now reading the
  other way round. So you can turn kiosk off and then hit Exit or Launch
  StageNow in the same visit, without holding BACK for another 10 seconds.

- "Launch StageNow" turns kiosk off, RELEASES THE SCAN ENGINE, and opens
  StageNow. Releasing the scanner is the important part: that is what was
  stopping StageNow from scanning. It finds StageNow by looking through the
  installed apps for a package name containing "stagenow", so it works on
  all three PDAs without anyone typing a package name. If it can't find it
  you get a toast saying so.

- "Exit app" turns kiosk off, releases the scanner and quits properly —
  same effect as Force stop, without going into Settings. Wild App is the
  launcher, so pressing HOME afterwards brings it straight back.

- "Android Settings" and "Change launcher" behave exactly as before.


INSTALLING

Hold BACK 10 s -> Leave, then install as usual. Same signing key, so it
installs over the top and your corrections, added addresses and scan
history are kept.

After installing, re-pick Wild App as the home app (any install clears it):
hold BACK -> More options is now the same list -> Change launcher -> pick
Wild App and choose ALWAYS, not Just once.


CONTENT

This APK also refreshes its built-in copy to the current published content
(repo build 156), restamped as 157. The built-in copy had been stuck on
build 137, so "Use built-in version" was giving very old behaviour. Baked
data is unchanged (build 131, a5195d2e, 24,102 rows) — it already matched
the repo, so there is no 2.1 MB re-download on first run.

No index.html upload is needed for this build.


ABOUT YOUR STAGENOW BARCODE

The repo's WildApp.apk is still build 137. If you want the barcode to
install THIS build, upload WildApp157.apk to the repo as WildApp.apk
first — the barcode always fetches whatever is at that URL.

Also: keep AppMgr on "Upgrade" for the two PDAs that still have the app
installed, so their data survives. The PDA you uninstalled from needs
"Install" once, then you can switch back to Upgrade.


VERIFICATION

- dex diff vs build 137: 8 new methods on MainActivity, 2 new inner
  classes, and exactly one changed body (showKioskDialog). Nothing else
  touched — confirmed by disassembling the built dex and diffing the
  smali text method by method.
- register check on all new methods: no negative locals, no writes into
  parameter registers.
- 7 of 11 entries byte-identical to the old APK.
- versionCode 157; platformBuildVersionCode left at 24.
- signed v1 + v2 + v3 with your key (SHA-256 2C:3A:BB:7A:B7:00:AF:19...).
- baked content: 11/11 checks pass, booted from the file actually inside
  the signed APK; 250-label render diff vs published build 156 = 0.
