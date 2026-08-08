# Wild App v1.71 — content build 67

OTA release. **No APK, no install.** Upload the two files, tap ⟳ Update, then tap the green button.

## Upload to `ashkayuk-cmd/wildapp-updates` (main)

| File | Upload as |
|---|---|
| `index.html` | `index.html` |
| `wild_data.json` | `wild_data.json` |

Both must go up — the app refuses a data file whose hash and row count don't match the stamps in `index.html`, so uploading one without the other does nothing.

`wild_data.xlsx` is **your master copy**, not for the repo.

Then on the PDA: **⟳ Update** → it turns green → tap it.

---

## 1. The Talbot Square label

Your scan was Lillian Penson Hall, 15-25 Talbot Square. Its real postcode is **W2 1TT** (FIRM); the label was printed **W2 1TR**, which is the 1–13 end of the square (14 SUSSEX GARDENS). Your data was right; the sender's was wrong.

It answered 14 SUSSEX GARDENS because the barcode destroyed both words that prove otherwise:

- `CITIW21TR` — the postcode was jammed onto the end of **CITI**, so it tokenised as the junk word `CITIW`. CITI appears on exactly one street in the whole round.
- `TA LBOT` — a stray space inside the word, so **TALBOT** never existed as a token at all, only `LBOT`.

Two fixes:

**Scan-text repair.** Before tokenising, a fused postcode is split off the word in front of it, and words a stray space has broken are rejoined. Both are validated against data the round actually holds — the postcode must be one that really exists, the rejoined word must be one the round really uses — so neither can invent anything. Two genuine neighbours ("HALL PLACE") can never be welded together, because at least one piece has to be junk on its own.

**A named building now beats a wrong postcode.** The old check asked the fuzzy matcher for its best record, and the fuzzy matcher scores a record by how much of *itself* the label explains — so "11, Talbot Square, W2 1TR" scored 1.0 (three words, all printed) and beat the Citi View record (LILLIAN and PENSON aren't on the label). That's right for ranking flats and wrong for judging a postcode. It now judges that question on evidence instead: a word the round confines to a street or two, printed on the label, that **nothing** at the scanned postcode can explain, pointing at records that agree on one walk and one different postcode, carrying the label's own number.

Your label now reads:

> **FIRM** — Paddington Citi View, Lillian Penson, 15-25, Talbot Square, W2 1TT
> ⚠ Postcode doesn't match this address — scanned W2 1TR
> ⚠ Or, by the label's postcode · W2 1TR → **14 SUSSEX GARDENS** (red, tappable)

Checked hard, because this path can override a printed postcode:

- All **24,145** records, label printed with its own correct postcode: the new path fired **0 times**. It never engages when the postcode agrees.
- 7,485 labels deliberately printed with a *wrong* postcode from the same street: it fired 2,068 times and gave the **right walk every single time** — 0 wrong.
- 294 sampled labels resolve **identically** to v1.70.

## 2. Tracking numbers are now ignored completely

A live scan of a bare S10 code (two letters, nine digits, two letters — `YR581784168GB`) is now a true no-op: **no beep, no buzz, no history entry, nothing on screen.** The scanner just keeps hunting for the address code, and whatever result is already up stays there and stays usable — Back and "Wrong walk?" still point at it.

Previously these announced "No postcode found". Case and stray spaces don't matter. An address that merely *contains* such a token still resolves normally; opening one from Recent scans still explains what it is.

## 3. Data

The two identical `15-25, Talbot Square, W2 1TT` FIRM rows are now one. **24,145 rows.**

The master workbook had drifted again — the copy baked into the v1.66 APK still had the old `22a Craven Hill Gardens W2 3AN`, the old `114 Queensway W2 4DB`, and the deleted Broadwalk CAFE row. The `wild_data.xlsx` here carries all of those plus the Talbot dedupe, and converts to the published `wild_data.json` with **0 differences, in the same order**.

## Tests

54 green: 44 in the v1.71 suite (the real label reproduced, both repairs, the tracking-code silence with tone/buzz/history all counted, regressions on W2 4RU / 4PR / 4DB / 3AN / 3AT / Hethpool / SOUTH CONCIERGE / Lancaster Hall / Junction Mews) + 10 gate checks (the app passes its own sanity gate after restamping, the loader is still rejected, hash and row stamps match the delivered data).

## Stamps

`THIS_BUILD_NUM=67` · `BUILD_NAME="v1.71"` · `REPO_DATA_BUILD=67` · `REPO_DATA_HASH="bdf6705e"` · `REPO_DATA_ROWS=24145` · `REPO_APK_BUILD=62` · `BAKED_DATA_BUILD=57`

`version.txt` and `content.json` are dead files — leave them alone.
