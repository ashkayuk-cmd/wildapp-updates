# Wild App — v4.47 (APK build 162)

Install over the top. Same signing key, so your corrections, added addresses
and scan history are kept.

Before installing: hold BACK for 10 seconds, choose **Leave**, then install.
The kiosk fights the installer otherwise.

After installing: **re-pick Wild App as the launcher.** Installing any APK
clears the preferred-home setting by itself.

---

## What changed

### 1. The Android status bar is now hidden while the app is open

This is what you asked for: the notification bar can't be pulled down while
Wild App is in front, but it comes straight back the moment you leave the app
or stop it. Nothing is changed system-wide, so leave the notification setting
**enabled** in your StageNow profile — the app handles it at runtime.

This is better than what was happening before. Previously the shade could start
to open and the kiosk would swat it, so you'd see it flash. Now there is no bar
to pull down in the first place.

One honest limit: on Android 8 a swipe from the top edge can still reveal the
bars briefly as a see-through overlay. Flags alone can't block that. The
existing kiosk bounce still catches anything that gets that far, so in practice
the two together should hold.

The navigation bar is deliberately **left alone** — HOME and BACK stay exactly
where they are. Only the status bar is hidden.

### 2. Battery readout in the top bar

Hiding the status bar takes the system battery icon with it, so the app now
shows its own, in the blue bar next to RESET:

- White fill at normal charge
- **Red** at 15% or below
- **Green with a ⚡** while charging
- Refreshes every 30 seconds, and whenever the app comes back to the front

To make room, the title text was shrunk — "WEBSITE VERSION" from 10.5px to 9px
and "wildapp.gt.tc" from 16px to 12.5px. The h1 itself is untouched, so
**tapping the title still takes you home** as before.

If the battery can't be read for any reason, the pill simply hides itself and
nothing else changes.

---

## Under the bonnet

**Native (classes.dex):** two new methods, nothing removed, nothing else
changed.

- `MainActivity.applyImmersive()` — sets the status bar hidden flags on the
  window. Called from `onCreate` and again on every focus gain, because Android
  clears these flags each time focus returns. Whole body is inside a
  try/catch so it can never take the app down.
- `MainActivity$ResetBridge.battery()` — reads the battery level and charging
  state and hands them to the web layer. Returns a "no reading" value on any
  failure rather than throwing.

**Content:** build 162 (v4.47), built on repo build 161 (v4.46), baked into the
APK. Builds up to 161 are all inside this APK, so no index.html upload is
needed for the app itself.

**Data:** unchanged — data build 131, hash a5195d2e, 24,102 rows. The hash port
was re-verified against the live stamp before building.

---

## Checks run

- Smali diff vs the installed build: exactly two added methods and two added
  call instructions. Everything else is label renumbering only.
- Both new methods pass the parameter-register check.
- Dex round-trip control confirmed byte-identical, so the diff above is real
  and not an artefact of reassembly.
- t47.cjs: 28/28. The same suite on build 161 fails the new checks and then
  throws reaching for the battery pill — the change reproducing.
- 250-label render diff vs build 161 across four label shapes: **0 differences.**
  The resolver is untouched.
- The app's own OTA sanity gate accepts this file; the loader is still
  correctly identified as the loader.
- versionCode 162, platformBuildVersionCode still 24.
- Signed v1/v2/v3, cert 2C:3A:BB:7A:B7:00:AF:19… (CN "Wild App").
- 7 of 11 entries byte-identical to the installed APK.

### One thing worth knowing

While building this, the restamping step corrupted the file on the first pass.
The pattern for `BUILD_NAME` ran into the line `html.indexOf("BUILD_NAME=")`
inside the update sanity gate and swallowed a chunk of live code, and the app
failed to boot in testing. This is the same trap as v1.47 — a sanity check that
inspects the app's own source. Restamping is now anchored to the `const`
declarations with a "must match exactly once" assertion, the gate function is
verified byte-identical to build 161, and every script block is syntax-checked
before packaging. Worth remembering for next time.

Testing also caught a real bug on the way: a failed battery read left the last
percentage frozen on screen instead of hiding the pill. Fixed.

---

## Note on the other two PDAs

They won't see this as an over-the-air update. The app learns the repo's APK
build from `REPO_APK_BUILD` inside the published index.html, which still says
157. Install this APK directly on all three, as usual.
