# Wild App — v1.52 (build 48)

Recents key no longer parks you outside the app, and the HOME key brings the app
back to its own home screen.

## Install it (in this order)

1. On the PDA, **hold BACK for 10 seconds** → tap **Leave**.
   (The kiosk fights the installer if you skip this.)
2. Open **WildApp.apk** and install. It installs straight over the top —
   your corrections and scan history are kept.
3. In your GitHub repo `ashkayuk-cmd/wildapp-updates`:
   - upload this APK as **WildApp.apk** (replace the old one)
   - set **version.txt** to **48**

`content.json` needs no change — build 48 ignores it.

## What changed

**Recents (the square key)**
The app now comes back in about 0.08 s instead of 0.35 s, with a second attempt
at 0.5 s in case the first is swallowed by the recents animation. If the recents
screen does stop the app completely, the return is 0.3 s instead of 0.7 s.

**HOME (the middle key)**
New: the app now listens for the HOME intent and resets itself to the home
screen — every sheet, picker and sub-screen closed, back to "Scan a barcode",
scrolled to the top. Nothing is lost: saved corrections, scan history and any
downloaded update are untouched, only the screen is cleared.

This only works while **Wild App is set as the device's Home app**
(Settings → Apps → Default apps → Home app). If it isn't, HOME goes to the normal
launcher and the app just pulls itself back to the front as before.

Coming back from recents deliberately does **not** reset the screen — your scan
result is still there. Only a real HOME press clears it.

**Also included** (in case you never uploaded them over the air)

- v1.50 — RESET APP button (two lines), and the W2 1PN "12 vs 1-2" barcode-error picker
- v1.51 — App version moved out of "Manage my corrections"; tap the version pill
  at the bottom left instead

## Not changed

The 10-second BACK-hold escape, the short-BACK behaviour, the scan trigger, the
address data (24,147 addresses), the icon and the signing key are all exactly as
they were. The bottom bar stays visible — hiding it would also make the HOME
button untappable, so if you want it gone that's a separate decision.

## Checks run before delivery

163 in total: 44 on the package itself (only the four intended files changed,
data byte-identical, versionCode 48, both signatures verify against your keystore,
same certificate as build 45 so it installs over the top, STORED entries aligned)
and 119 in the browser tests (15 new ones for the HOME reset, plus the update,
version-screen, RESET button and Junction Mews suites re-run against the app file
inside this APK).
