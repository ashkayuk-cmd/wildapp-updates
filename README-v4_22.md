# Wild App — v4.22 (APK build 136)

**One file, one install.** No `index.html`, no `wild_data.json` — everything is
inside the APK.

## Install

1. Hold BACK for 10 seconds → "Leave".
2. Install `WildApp.apk`. Same key, so corrections and history are kept.
3. Set WildApp as the default launcher again.
4. Upload the same file to the repo as `WildApp.apk`.

---

## Why the beep wasn't firing

You were scanning real parcels, not your walk, with today's walk set — and
getting nothing. That ruled out a device or volume problem straight away, so I
went looking in the code that runs when a **scan doesn't resolve to a walk on
its own** and shows you a list to tap instead.

This round has postcodes that cover more than one walk — 15 of them. Any time
a scan lands on one of those (or on a "closest match" list when nothing
matches exactly), you get a **card list to tap**, and picking a card was
never wired to make a sound at all — just the walk name and, if you have
speech on, it saying the name. That gap has been there since the tap-to-pick
screens were built, long before the harsh-beep feature — it just never
mattered until there was a beep to be missing.

So the harsh blats **did** fire on a straightforward scan that resolved to one
walk by itself. They went silent specifically on the scans that needed a tap
first — which, on a round with this many split postcodes, is often.

**Fixed in three places** — the "also possible" card under a result, the
plain "possible walks" list, and the "closest addresses" fallback when
nothing matches exactly. All three now sound the tone (and vibrate) the
moment you tap a card, exactly as if that walk had come up directly from the
scan. Picking your own walk still plays the ordinary quiet beep and the green
"✓ YOUR WALK" — only picking someone else's gets the three blats.

## Also in this build

**The top and bottom bars no longer flash white** when the app restarts and
you scroll. They were hidden by a stylesheet that only takes effect once it
loads — for a moment at boot they're visible, and scrolling could show/hide
them again. They're now hidden directly in the page markup itself, before any
stylesheet runs, so there's nothing to flash.

**The spoken "Not your walk" prefix is gone** — the voice just says the walk
name now. The red banner and the harsh beep still warn you; only the spoken
words changed, as asked.

---

## How it was checked

- **Reproduced the exact failure first**, before touching any code: scanned a
  shared postcode through the real render pipeline with today's walk set,
  tapped the OTHER walk's card, and got silence — the bug, caught live rather
  than guessed at.
- Same reproduction confirms the fix: **three 880 Hz blats** now play on that
  tap. A second check confirms tapping your **own** walk off the same list
  still gets the ordinary tone and the green banner — nothing over-corrected.
- **61-test suite unchanged and still green** on the file actually inside this
  APK.
- **250 and 400-label render diffs — 0 differences** against build 135. Only
  the tone changed; nothing about how a result looks moved.
- **7 loader tests**, and the baked app checked against the **real repo as it
  stands today** — still correctly claims no update, no phantom APK, no data
  re-download.
- `classes.dex` byte-identical to build 115. No native change.

## Stamps

`versionCode 136` · `THIS_BUILD_NUM=136` · `BUILD_NAME="v4.22"` ·
`BAKED_DATA_BUILD=136` · `REPO_APK_BUILD=136` · `REPO_DATA_HASH="a5195d2e"` ·
`REPO_DATA_ROWS=24102` · loader `BAKED=136`

`WildApp.apk` — 1,119,153 bytes
