# Wild App — v1.77 (build 73)

OTA release. **No APK, no install.** One file to upload.

## Upload

| File | Upload as |
|---|---|
| `index.html` | `index.html` |

To `ashkayuk-cmd/wildapp-updates` (main), replacing the old one. **Nothing else** —
the addresses haven't changed (still 24,149), so `wild_data.json` stays as it is.

Then on the PDA: **⟳ Update** → it turns green → tap it.

---

## What I built on

Worth knowing, because it isn't what it looks like. The `index.html` sitting in
your repo says **build 71 (v1.75)** — but the APK on your PDA is **v1.76 / build
72**, and a downloaded copy is only used when its number *beats* the APK's. So
the repo copy is dead weight: your PDA ignores it and runs the app baked inside
the APK.

That baked copy is the real "current app", and it's what I changed. This file is
build 73, so it beats 72 and will actually be used.

One consequence: the repo copy also carried a change that made **every** 1D
barcode register on screen. That change has never run on your PDA, and v1.76 now
handles 1D codes in the scanner itself (drop tracking numbers, drop 1D codes with
no postcode, keep hunting). So I deliberately left it behind rather than
reviving it — if you *did* want 1D codes shown again, say so and I'll do it
properly on top of the native filter.

---

## 1. W2 1PN always offers the firm

Any label with **W2 1PN** on it now keeps the firm on screen as a tappable option
under whatever the app worked out:

> **11 NORFOLK** — 5, Junction Mews, W2 1PN
> ⚠ Also possible · the firm at 1-2
> **FIRM** — 1-2 Junction Mews, W2 1PN (firm)  *(red, tappable)*

Tap it and it becomes the answer like any other pick — speaks the walk, keeps
Back and "Wrong walk?" working.

That covers the ones that used to give you 11 NORFOLK and nothing else: a bare
`W2 1PN`, a `5`, a `10`, an `11a`, or a label with no number at all.

**Three things I deliberately left alone**, because they already answer FIRM or
are your own ruling:

- **1, 1-2 or 2** on the label → still snaps straight to FIRM, no extra card.
- **12** → still the "⚠ Barcode error" two-button picker (FIRM 1-2 / 11 NORFOLK 12).
- A **correction you've saved yourself** for a W2 1PN address still wins outright.

This is the handheld screen — the one the PDA trigger feeds. The phone camera
screen is unchanged.

## 2. The other walks on a street

Tap a walk name → the streets list now shows, under each street, a blue tag for
every **other** walk that delivers on it:

> **Southwick Street** 20
> ⚠️ also on 13 RADNOR · 135

The number on the tag is how many addresses that other walk has there. Tap the
tag and you get *that* walk's streets, with **Back to 11 NORFOLK** to step back
where you came from. The header now reads e.g. `18 streets · 575 addresses · 2 shared`.

FIRM counts as a walk here — a couple of firm doors on your street matters as
much as a split with another round.

From your data:

- **Walk 11** has two shared streets: Southwick Street (13 RADNOR, 135 addresses)
  and Norfolk Square (one FIRM door).
- Your Delamere example is **Delamere Terrace** — walk **2 WOODCHESTER** has 138
  addresses on it, walk **3 JOHN AIRD** has 7.
- Across the whole round, **48 of 260 streets** are shared. Only 2 walks have no
  shared street at all.

Hardware BACK still exits the streets screen straight to the scan (as it did
before) — it's the on-screen Back button that retraces the walk-to-walk chain.

---

## Checked before delivery

- **91 tests** on this exact file: every W2 1PN shape, the three paths that must
  *not* change, the tap-through, and — for the streets index — a brute-force
  parity sweep proving the rewritten street counts match the old ones on **all 36
  walks**, plus a check that the `wild_data.json` already in your repo still
  matches this build's data stamps (hash and row count).
- **10 end-to-end tests**: the build you're running downloads this file, judges
  it, caches it; the APK's real loader then boots it and both new features work
  after the handover; and it doesn't wrongly claim another app update afterwards.
- **260-label render comparison** against the app you're running now: exactly
  **2 differences**, both the intended W2 1PN cards. Everything else — Talbot
  Square, the concierge labels, Lancaster Hall, tracking codes, the not-in-W2
  screen — resolves identically.

## One thing you'll see

After the app update lands, the ⟳ button may go green once more, for the
**addresses**. Tap it — it's the same 24,149 addresses you already have. It's
there on purpose: if this file ever lands on a PDA still running an older APK,
that device must not be left on the old address list (the concierge rows and the
Queensway fixes would silently vanish). One redundant tap is the cheaper mistake.

`index.html` — 564,349 bytes, sha256 `941e43f34647602c…`
Stamps: `THIS_BUILD_NUM=73` · `BUILD_NAME="v1.77"` · `REPO_DATA_BUILD=72` ·
`REPO_DATA_HASH="e71e95e7"` · `REPO_DATA_ROWS=24149` · `REPO_APK_BUILD=72` ·
`BAKED_DATA_BUILD=57`
