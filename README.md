# Wild App — v1.53 (build 49)

The recents key can no longer take you out of the app.

## Install it (in this order)

1. On the PDA, **hold BACK for 10 seconds** → tap **Leave**.
2. Open **WildApp.apk** and install. Installs over the top — corrections and
   scan history are kept.
3. In `ashkayuk-cmd/wildapp-updates`: upload this APK as **WildApp.apk** and set
   **version.txt** to **49**.

## First launch — one prompt to accept

The app now pins itself to the screen. The first time it does this, Android asks
something like **"Pin screen?"** — accept it. After that:

- **Recents (square)** does nothing. No minimise, no bounce back.
- **HOME (middle)** also does nothing — that's the trade-off, Android blocks both.
- **BACK** still works exactly as before, including the 10-second hold.

**If nothing changes**, screen pinning is switched off on the device:
Settings → Security → **Screen pinning** → on, then restart the app.

## Getting home without the HOME key

**Tap "Handheld Scanner" at the top of the screen.** That does what the HOME key
did in v1.52: closes any sheet or picker and puts you back on the idle
"Scan a barcode" screen. Nothing is lost — corrections, history and downloaded
updates are untouched.

## Installing updates from now on

Unchanged: **hold BACK 10 seconds → Leave**. That turns kiosk mode off *and*
releases the pin, so you can reach Settings and the installer as usual. The pin
comes back on the next launch (or when you turn kiosk mode back on).

Downloading an update from the app also releases the pin automatically.

## Why not the Zebra route

I said I'd try disabling just the recents button through Zebra's MX settings so
HOME would keep working. I've left that out: it needs exact MX setting names I
can't verify from here, and as far as I can tell MX only offers hide-the-whole-
navigation-bar, which kills HOME anyway. Guessing would have shipped an APK that
silently did nothing. Pinning is the mechanism that actually works, so that's
what this build uses.

## Checks run before delivery

169: 50 on the package (only the four intended files changed, address data
byte-identical, versionCode 49, both signatures verify against your keystore,
same certificate as build 45, pin code present and released in all the right
places) and 119 in the browser tests, including 4 new ones for the tap-the-title
shortcut. None of this can prove the pinning behaves on the device — that part is
untested, as agreed.
