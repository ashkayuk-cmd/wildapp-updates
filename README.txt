WILD APP — v1.66 (build 62)
===========================

WHAT'S IN IT

  The volume keys are now handled by the app itself.

  Pressing volume up or down used to open Android's volume panel. That panel
  is a separate window, so the app lost focus to it — and the kiosk answers a
  loss of focus by shutting system windows and pulling itself back within a
  tenth of a second. That is the flashing at the top and bottom, and it is
  also why the slider "did nothing": it was being swatted away before you
  could drag it.

  Now the app takes those keys itself, moves the volume directly, and draws
  the level on its own screen for a second. Android's panel never opens, so
  there is nothing to flash and nothing to fight.

  If the bars still flash when you are NOT pressing the volume keys, tell me
  when it happens — that would be a second, separate cause.

  Also carried in (these were the over-the-air release you may not have
  uploaded yet — they are baked into this APK, so installing gets you them
  either way):

    - Type a postcode — a button on the scanning screen, with its own
      keypad and live suggestions from the addresses on your round.
    - Sound — a working level for the scan beeps: slider, + / -, and a test.
      Off silences the beeps and keeps the buzz.


TO INSTALL

  1. Hold BACK for 10 seconds, then tap "Leave".
  2. Open WildApp.apk and install over the top.
     Same signing key as always, so your corrections, added addresses and
     scan history are all kept.
  3. Upload this file to the update repo as WildApp.apk.

  Nothing to edit. version.txt and content.json are no longer read by the
  app — leave them in the repo as they are.


AFTER INSTALLING

  The pill at the bottom-left should read: v1.66 - build 62
  Tap it, and "Installed app (APK)" should also say build 62.

  Press a volume key: you should see the level appear on the app's own
  screen, and the bars should stay put.


CHECKED BEFORE SENDING

  Signed with your key (fingerprint 2C:3A:BB:7A...), versionCode 62.
  Everything except the app file, the loader, the addresses, the manifest
  and the code is byte-for-byte the one you are running now.
  250-odd automated checks, including the address lookups run against the
  real 24,147-address data — identical results to the build you have.
