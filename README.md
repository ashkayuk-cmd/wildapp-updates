# WildApp.apk — build 241 / v7.6 (versionCode 200)

Adds a **Wi-Fi setup** screen so you can set the RM network on a PDA without StageNow.

## What changed
- New native bridge method `WildKiosk.wifiApply(...)` — pushes a Wi-Fi config to the
  device through EMDK MX, the same path the notification pull-down toggle already uses.
  This is the only change to the app's code (verified: exactly one method added, nothing
  else in the dex touched).
- New **Wi-Fi setup** row in the gear (⚙️) → Options menu.
- versionCode bumped 199 → 200. Signed with your `wild` key (fingerprint
  `2C:3A:BB:7A:B7:00:AF:19…`), so it installs **over the top** — corrections and history
  are kept. v1+v2+v3 signatures, zipaligned.

## Install
Same as always — upload `WildApp.apk` to the repo and let StageNow pull/install it, or
sideload. No `index.html` to upload this time: the new app content (build 241) is baked
into the APK, and because it's build 241 it takes over from the downloaded build 240 your
PDAs are currently running, automatically, the moment the APK is installed. Install it on
all three PDAs.

(If StageNow does the install, the usual scanner note applies: Wild App holds the EMDK
engine, so use the Back-button menu → Launch StageNow, or force-stop the app first.)

## How to use it
1. Tap the gear ⚙️, enter PIN **1984**.
2. **Wi-Fi setup**.
3. Type the network name (SSID) and the password. "Show password" lets you check it.
4. **Save & connect**. The status line shows **SUCCESS** (accepted) or an error straight
   away — test it indoors, not mid-round.
5. "Reconnect automatically on start-up if offline" is on by default: if a PDA boots and
   isn't online, it re-pushes the saved network once. It does nothing when already online.

The SSID + password are saved **on that device only** (localStorage). Nothing goes to the
repo or anywhere else.

## Things to know
- **I couldn't test the MX Wi-Fi call from here** — there's no device in the build sandbox.
  The schema is Zebra's documented WPA2-PSK MX Wi-Fi doc (version 8.4), and the app tries a
  short ladder of CSP versions and remembers whichever your TC56 accepts (same trick the
  pull-down uses). The on-screen status is your confirmation. If it says FAILURE, the most
  likely cause is a wrong password; if it says "MX/EMDK not ready", wait a moment and retry.
- **This is WPA2-Personal (a network name + one password).** If the RM network is actually
  WPA2-**Enterprise** (a username *and* password / 802.1X), this form won't connect — tell
  me and I'll build the enterprise version.
- **Getting future updates still needs the device online first** — the auto-reconnect only
  helps a PDA rejoin a network it already has saved. If RM changes the password, type the
  new one in on each PDA.
- As agreed, this does **not** include the "try a list of passwords" behaviour.
