# Wild App — build 104 (v2.9)

**15 August 2026 · content update only — no APK, no install.**

This one file carries every change from build 99 onwards. If you never uploaded
99–103, uploading this covers all of them at once.

---

## What to upload

| File | Where | Needed? |
|---|---|---|
| `index.html` | repo root, replacing the existing one | **yes** |
| `wild_data.json` | repo root, replacing the existing one | only if you haven't already uploaded it today |

Both go in `ashkayuk-cmd/wildapp-updates`, on `main`, under exactly those names.

Two things that have caught us out before:

- **The file must be named `index.html`.** A mobile upload that lands as
  `index (1).html` publishes nothing.
- **Press the green "Commit changes" button.** Choosing the file isn't the
  upload.

Then on the PDA: ⚙️ Settings → App version → **Check now**. The first check
after a publish is done by the *old* code, so if a new button doesn't appear
straight away, check once more.

Stamps in this file: build **104**, name **v2.9**, data build **100**, data hash
**b3956e1b**, 24,149 rows, APK build **97**.

---

## What's new

### Walks (⚙️ Settings → Walks) — build 99
Every walk in the round, in order with FIRM last, showing its street and
address counts. Tap one for its streets.

Your A‑Z list's street column is now used: a named building sits **under the
street it stands on** (`🏢 ASTLEY HOUSE · BRINDLEY HOUSE`) instead of dropping
into the loose list at the bottom. That list went from 325 entries across all
walks down to 62, and those 62 show their street when the sheet gave one.

### Street search — build 104
A search box on the Walks screen. Type two letters of a street and it tells you
which walk has it, with the walks as tappable chips. Southwick Street comes back
as 11 NORFOLK and 13 RADNOR.

### Data fix — build 100
Roman Catholic Church of Our Lady, Queen of Heaven, 4a Inverness Place,
W2 3RS → **23 INVERNESS**.

Knock-on effect worth knowing: W2 3RS is now 7 Queensway addresses on walk 24
plus this one church on 23, so a label carrying **only** the bare postcode will
offer both walks. Anything naming 4a, Inverness Place or the church resolves to
23 outright.

### Scans per walk (⚙️ Settings → Recent scans) — builds 101, 102
A tally above the scan list: every walk you've scanned, busiest first, counted
over your whole saved history rather than the 40 shown.

Tap a walk for **just its scans**, each with the raw barcode/QR code printed
underneath. Tap one of those to replay it.

Also on that screen: **Clear recent scans**, two taps (the first arms it, and it
disarms itself after 4 seconds).

### Didn't resolve — build 104
The "N scans didn't land on a single walk" line is now a button. It lists the
pick-lists, no-matches and out-of-area labels with their raw codes, and has a
**Copy the list** button. Send a week of those over and the gaps can be fixed in
the data properly.

### Split postcodes (⚙️ Settings → Split postcodes) — build 104
The app now finds the postcodes that cause pick-lists, instead of you finding
them on the doorstep.

16 of the round's 1,086 postcodes go to more than one walk. Fourteen are marked
**amber** — a tiny minority of addresses on a *different street* from the rest,
which is the shape that caused the Leinster/Craven and 114 Queensway problems.
W2 3SS (one Seymour Street among 82 Queensborough Terrace) and W2 3RS are both
there.

W2 1NJ and W2 5PN aren't flagged: those are the same street split between a
walk and FIRM, which is honest, not an error.

Tap any postcode for every address at it with its walk.

### Back button — build 103
BACK used to send you home from any screen, however deep you'd gone. It now
steps back one screen at a time: Options → Walks → walk 11 → its shared walk 13
takes four presses to get home, in the order you came.

The correction flow deliberately still closes straight to the result — a stray
press shouldn't drop you back into a half-finished fix. A new scan clears the
trail.

---

## Not changed

The resolver, the scanner, the correction flow, sound and voice are all
untouched. A 250-label comparison against the running build shows no difference
in what a scan reports — every change here is a screen you go looking for.

Your saved corrections, added addresses and scan history all survive this
update.
