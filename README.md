# Wild App v4.17 — build 131 — Today's walk + the Cleveland Terrace fix

**Upload BOTH files to the repo: `index.html` and `wild_data.json`.**
No APK. The data file is only accepted when the stamps inside `index.html`
match it, so the two must go up together.

Watch the filenames — if your phone saves either as `index (1).html` or
`wild_data (1).json` the upload does nothing — and press the green
**Commit changes** button.

This replaces the build 130 file from earlier; it contains everything in it.

| | |
|---|---|
| Code build | 131 (v4.17) |
| Data build | 131 — 24,102 rows, hash `a5195d2e` |
| APK | still 115, unchanged |

---

## 1. The data fix

**22, Cleveland Terrace, W2 6QH** moved from **18 GLOUCESTER** to
**16 WESTBOURNE TCE**. One row changed, nothing else — 19, 20, 25, 25a, 27a,
27b and 27c were already on 16.

The knock-on: **W2 6QH is no longer a split postcode.** A bare W2 6QH label
used to give you a two-walk picker; it now answers 16 WESTBOURNE TCE outright.
It has also dropped off the Split postcodes screen.

> Your master `wild_data.xlsx` still has this row on 18, along with the
> duplicates and deletions from v4.1/v4.3/v4.5. If you ever re-upload the
> master it will quietly undo all of them. Worth letting me rebuild it to
> match.

## 2. Today's walk (carried over from build 130)

**⚙️ Options → 📍 Today's walk.** Pick the walk you're on. Every scan that
lands on a different walk gets a red band across the top of the result:

> ⚠️ **NOT YOUR WALK**
> you're on 12 WESTCLIFFE

and the voice says **"Not your walk"** before the walk name, so you don't have
to be looking at the screen. A scan that *is* yours gets a thin green
**✓ YOUR WALK** strip, so you can see at a glance the warning is switched on.
The screen also shows the day's tally — scanned today, yours, not yours.

**It clears itself at midnight**, so yesterday's walk can never shout at
today's mail. Tapping the walk you already have set clears it too, as does the
Clear button. With no walk set, the app behaves exactly as build 129 did.

---

## How it was checked

- **t54.cjs — 25/25** on the corrected data. The same suite on build 129 fails
  24 of the 25, so it is testing the change.
- **250-label render diff, build 130 vs 131 on the same data — 0 differences**,
  so the data build changed no code.
- **250-label render diff vs build 129 — 0 differences** with no walk set;
  with a walk set, the banner appears on 223 and, once the banner element is
  removed, the markup is identical on all 250.
- Data file re-hashed with a port of the app's own `wildHash`: `a5195d2e`,
  24,102 rows — matching the stamps in `index.html`, which is what the app
  requires before it will accept a data download.
- Build 115's `liveHtmlLooksSane` accepts the file; loader gates pass.

## Still waiting on you

- **W2 1HQ** — The Premier League, 57 North Wharf Road (FIRM) vs Tournament
  House, Praed Street (12 WESTCLIFFE). Same building or two?
- **W2 1LA** — 22 North Wharf Road rows are FIRM, but The Gym Group (33-35) and
  Heist Bank (5) on the same street sit on 6 SHELDON. Should those be FIRM?
- **W2 1RH** — Caffe Nero Paddington Station on 11 NORFOLK, while 18 Praed
  Street rows are on 12 WESTCLIFFE.
- **W2 5PN** — 2a/2b/2c Shrewsbury Mews on 32 BRUNEL, the rest of the mews on
  30 CHEPSTOW.
- **W2 5TL** — 86 Senior Street on 1 ALFRED, the school on 2 WOODCHESTER.

---

# wild_data.xlsx — your master workbook, rebuilt

**This one does NOT go to the repo.** It replaces the `wild_data.xlsx` you keep
and edit. The repo still only ever gets `index.html` and `wild_data.json`.

It is the shipped data exactly — 24,102 rows — so the two are back in step and
a future upload of the master can't quietly undo anything.

## What it puts right

Your old copy was 24,149 rows. The 47 differences, all of them changes already
live in the app:

| | |
|---|---|
| 45 rows | duplicate addresses removed (v4.3) — 41 addresses that appeared 2 or 3 times: Buck Hill Lodge, Rangers Cottage, Flat 32 Saxon Hall, 84 Kensington Gardens Square, most of Paddington Square, and the rest |
| 1 row | `24, Seymour Street, W2 3SS` deleted (v4.1) |
| 1 row | `Buckhill Lodge, Bayswater Road` deleted (v4.5) — the misspelled second copy of Buck Hill Lodge |

Plus the two walk changes: `22, Cleveland Terrace, W2 6QH` → 16 WESTBOURNE TCE
(today), and the Inverness Place church → 23 INVERNESS (v2.5).

## Format

Same as before — **Data** and **Walks** sheets, columns
Address | Walk | Street | Folder | Subfolder, the Walk dropdown on column B
off the `WalkList` name, autofilter, header row frozen. The four rows carrying
a Folder value (Paddington Station, Hallfield Estate, Bayswater Road, Kingdom
Street) are carried across untouched.

**Checked:** converted the workbook back the way the app does and it reproduces
`wild_data.json` byte-for-byte — same hash `a5195d2e`, same 24,102 rows. Opens
clean in LibreOffice.

> One thing to check at your end: if your master also had a **Walk Order**
> column and a **Sorted View** sheet, they aren't in this one — the copy I had
> to work from (the one baked into the APK) doesn't carry them. Send me your
> real master and I'll fold the same 47 changes into that instead.
