# Brightness setting — build 187 / v4.72

## What's new
A new "Brightness" row in Settings (sorted alphabetically, sits between
"Back button menu" and "Recent scans"):
- **Auto brightness** toggle — on (default) leaves the screen at whatever the
  device itself is set to; off lets you fix it manually.
- **Slider (1–100%)** — appears when Auto is off, applies live as you drag,
  and is remembered across restarts.

## Why this needed a full APK, not just an OTA push
Screen brightness can only be set through the Activity's Window, which isn't
reachable from JavaScript — it needed a new native bridge (`WildBrightness`,
alongside the existing `WildAudio`/`WildTTS`/etc.) exposed into the WebView.
That's a dex-level change, so this one has to be installed by hand like the
last one.

## Deploy
1. **Install `WildApp.apk` on the device** — same signing cert, same
   `versionCode` 170 (only the OTA-layer `THIS_BUILD_NUM` moved, to 187, same
   scheme as every other content-only release), installs as a normal update.
2. **Upload `index.html`** to `ashkayuk-cmd/wildapp-updates` on GitHub — this
   is the same content now baked into the APK, so devices that get the OTA
   push before the new APK will simply not see the Brightness row yet (the
   Settings menu hides it automatically when the bridge isn't present), and
   devices with the new APK but an older OTA copy will get it back as soon as
   they pick up this OTA. Either order is safe.
   `wild_data.json` is untouched — no need to re-upload it.

## Compatibility
Feature-detected the same way `WildAudio` already is: if `window.WildBrightness`
doesn't exist (older wrapper, or opened in a browser during preview), the
Settings row just doesn't appear — no dead controls, no error.

## Also carried over from the crash-fix build
The `requestAdmin()` register-collision fix, the on-screen crash display
around `onCreate`'s WebView setup, and the earlier crash-logger hook are all
still in this build.
