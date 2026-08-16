# Wild App — v4.0 (APK build 115)

**Install this one. It replaces both v3.8 and v3.9** — if you haven't installed
either yet, good, you only need this. Everything from those two is in here.

One file, one install, one upload. No `index.html`, no `wild_data.json`.

---

## Install

1. On the PDA: **hold BACK for 10 seconds → "Leave"**.
2. Install `WildApp.apk`. Same key, so corrections, added addresses and scan
   history are all kept.
3. Upload the same file to `ashkayuk-cmd/wildapp-updates` (main) as
   `WildApp.apk`.
4. **Set WildApp as the default launcher again** — installing over the top
   clears that by itself, every time.

---

## The Back button skipping Options

You were right, and it was skipping it everywhere, not just those two screens.

The app keeps a trail of the screens you've walked through, so Back can retrace
them one at a time instead of jumping home. **The Options sheet was never
recorded on that trail.** It isn't a normal screen — it's a full-page sheet laid
over the top, and it removes itself the moment you tap a row. So by the time
Walks or Recent scans opened, the trail was empty, and Back had nothing to step
back to except the scan itself.

Options now records itself like any other screen. So:

```
scan → ⚙️ → Walks → walk 11's streets
```

takes three Back presses to get home, in the order you came:
streets → walks list → **Options** → the scan.

Two details I made sure of:

- **Backing into Options clears the screen underneath.** Otherwise closing the
  sheet again would reveal the Walks list you'd just left, and you'd be stuck in
  a loop.
- **A scan is put back, not thrown away.** If you were looking at a result when
  you opened Options, backing all the way out returns you to that result, not to
  the blank "Scan a barcode" screen.

### Two more that had the same fault

- **Sound** now comes back to Options too. Same cause — it never joined the
  trail.
- **Recent scans' own Back button** said just "Back" and went to the scan,
  unlike Walks and Split postcodes which already said "Back to Options". It now
  says and does the same as those two.

**App version and Walk corrections are deliberately left alone.** Both sit
behind the PIN, and putting them on the trail would let a Back press walk into
them without asking for it again.

---

## Also in this build

Everything from the two APKs before it:

- **The crash fix** (v3.8) — `onRenderProcessGone` is answered, so a dead page
  process restarts the app instead of Android killing it.
- **The crash log** (v3.9) — every start and every crash written to
  `Android / data / uk.wild.app / files / wild-crash.txt`, readable with your
  file manager. Send it over after the next crash.
- **Content and address data brought up to date inside the APK** — it had been
  carrying build 97 and the pre-fix Our Lady, Queen of Heaven row.
- **The loader's build number** unstuck from 86.

---

## Checked before delivery

- **20 back-journey tests** driving the real screens: Options → Walks → back →
  Options → back → home; the same for Recent scans; three levels deep stepping
  back one at a time; Sound; a scan being restored underneath; the relabelled
  Back button; and six round trips to prove the trail doesn't grow each time.
- **The same suite run against published build 112 fails exactly the fixed
  cases** and then throws when it reaches for an Options row that never
  reopened — the bug reproducing, which is the confirmation that mattered.
- **160-label render comparison** against build 112: **0 differences**, nothing
  threw. The resolver, scanner and correction flow are untouched.
- **`classes.dex` is byte-identical to build 114's** — no native change in this
  build at all, so the crash fix and logger are exactly as verified before.
- **19 boot tests** and **11 loader tests** re-run against the baked file.
- Change footprint 10 hunks. **One near-miss worth recording:** relabelling
  Recent's Back by matching its markup would have hit **12 unrelated buttons**
  that share the identical HTML — an assertion caught it, and the change is
  keyed on the element id instead.
- 6/6 untouched APK entries byte-identical, entry order preserved, versionCode
  115, `platformBuildVersionCode` still 24, signed v1/v2/v3 with your key
  `2C:3A:BB:7A:B7:00:AF:19…`.

## Stamps

`versionCode 115` · `THIS_BUILD_NUM=115` · `BUILD_NAME="v4.0"` ·
`BAKED_DATA_BUILD=115` · `REPO_APK_BUILD=115` · `REPO_DATA_BUILD=100` ·
`REPO_DATA_HASH="b3956e1b"` · `REPO_DATA_ROWS=24149` · loader `BAKED=115`

`WildApp.apk` — 1,131,441 bytes, sha256 `6cbfe537361c1c60…`
