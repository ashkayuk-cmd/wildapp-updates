# Wild App — v3.8 (APK build 113)

**This is an APK.** Install it, then upload the same file to the repo.

One file only. You do **not** need to upload `index.html` or `wild_data.json` —
build 113 is baked inside this APK and is newer than anything in the repo.

---

## Install

1. On the PDA: **hold BACK for 10 seconds → "Leave"**. The kiosk fights the
   installer otherwise.
2. Install `WildApp.apk`. Same signing key as always, so it goes over the top —
   your walk corrections, added addresses and scan history are all kept.
3. Upload the same `WildApp.apk` to `ashkayuk-cmd/wildapp-updates` (main),
   replacing the old one, under exactly that name.

**Then set WildApp as the default launcher again.** Installing any APK over an
existing one clears Android's preferred-home setting on its own — that part is
normal and will happen every time you install a build, fix or no fix.

---

## The crash

`WildWebViewClient` never overrode `onRenderProcessGone`.

On Android 8 the web page runs in a **separate process** from the app. When that
process dies — either it genuinely crashed, or Android killed it to reclaim
memory while the app was in the background — Android calls `onRenderProcessGone`
to ask the app what to do about it. If nothing answers, the default answer is
"I can't handle this", and **Android kills the whole app**. That is the crash:
not a fault in your code, a question nobody was answering.

It now answers. The override calls the `restartApp()` you already have — the one
behind RESET APP: toast "Restarting app…", release the scanner, schedule the
relaunch, exit — and tells Android it's handled. So instead of the app vanishing,
you get "Restarting app…" and it comes straight back.

**Everything else was audited and is already covered.** `deliverScanDirect` is
wrapped in try/catch, the scanner sink hops to the UI thread before touching
anything, both JS bridges are thread-safe, and scan history is capped at 200
entries so it can't grow until storage bursts. A JavaScript error breaks one
screen, not the process. This was the only unguarded path left.

### About the default launcher

Two separate things were making it not stick, and only one of them is a bug:

- **The crash.** A home app that dies gets dropped back to the system launcher.
  That's fixed here.
- **The reinstall.** Installing over the top clears the setting by itself. That
  isn't fixable and will keep happening with every build.

So: re-pick it after this install, then see whether it holds. If it still drops
on its own days later with no install in between, the crash wasn't the cause and
I'll need to add crash logging to find out what is — say the word and I'll build
it. Right now there's no way to read a crash off the device without a PC.

---

## Also in this build (no behaviour you asked for changed)

### The content is brought up to date

The APK was still carrying build 97 (v2.2) inside it. It now bakes **build 113**,
which is the v3.7 you're already running from the repo — the new top bar, the
walks street search, the "📍 yours:" street lines, all of it. Same file, only the
build stamps differ.

### The address data inside the APK was six days stale

The APK's built-in copy still had **Our Lady, Queen of Heaven, 4a Inverness
Place** on **24 QUEENSWAY**. It's on **23 INVERNESS** now, matching the repo.
That only mattered if you'd ever dropped back to the built-in version — it would
have quietly given you the old answer for that one address. Fixed, and it saves
a 2.1 MB download after the install because the built-in data is now current.

### A loader bug worth knowing about

The loader that decides "built-in copy or downloaded copy?" was comparing against
**86** — a number left behind three APKs ago. It should have said 97. The effect:
a downloaded copy *older* than the app baked into the APK would still win.

It says 113 now. After this install the PDA runs the baked 113 straight away
rather than falling back to the 112 it has cached. Your cached copy isn't
deleted, just correctly judged as older.

---

## Checked before delivery

- **Dex diff against the installed build 97: exactly one added method**,
  `onRenderProcessGone`. 134 methods before, 135 after, **zero changed bodies** —
  the scanner, the sound values, the kiosk, `restartApp` itself, all untouched
  byte for byte. (The disassemble/reassemble round-trip was verified against the
  original first, so the assembler isn't quietly rewriting anything.)
- **19 boot tests** on the exact `app.html` inside this APK: stamps, the v3.7 top
  bar (the `h1` is still there — it's your only way home), gear before RESET,
  the 52px bar, the walks search and street-summary code, and the church row
  reading 23 INVERNESS.
- **160-label render comparison** against the published build 112 across four
  label shapes (own postcode / wrong postcode / stray space / out of area) —
  **0 differences**, nothing threw. How labels resolve has not moved.
- **11 loader tests**, including the control that proves the old loader really
  did prefer a stale copy: no cached copy boots built-in; 112 and 113 both lose
  to baked 113 without being binned; a genuinely newer copy still boots; the
  watchdog, a truncated copy and a copy of the loader itself are all still
  refused.
- **6 of 6 untouched APK entries byte-identical**; entry order preserved;
  `resources.arsc` and the launcher icon still STORED; versionCode 113;
  `platformBuildVersionCode` still 24.
- Signed with your key — `2C:3A:BB:7A:B7:00:AF:19…`, verified v1/v2/v3.

## Stamps

`versionCode 113` · `THIS_BUILD_NUM=113` · `BUILD_NAME="v3.8"` ·
`BAKED_DATA_BUILD=113` · `REPO_APK_BUILD=113` · `REPO_DATA_BUILD=100` ·
`REPO_DATA_HASH="b3956e1b"` · `REPO_DATA_ROWS=24149` · loader `BAKED=113`

`WildApp.apk` — 1,131,441 bytes, sha256 `56657d89dcf1a099…`

`version.txt` and `content.json` in the repo are dead files — leave them alone.
`assets/wild_data.xlsx` inside the APK is also never read; it's still the old
master copy and was left alone deliberately.
