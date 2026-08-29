# Wild App — v4.51 (content build 166)

**Upload `index.html` to the repo. No APK, no install.**

All three PDAs pick it up on their next check. Upload as exactly `index.html`
(not `index (1).html`) and press the green **Commit changes** button.

The change won't show on the *first* check after publishing — the old code runs
that check. Check again.

---

## What changed

### 1. New wording on the scanning screen

Now three short paragraphs:

> The PDA reads the **postcode** from the QR code and shows the walk. It is
> usually correct, but **not 100% accurate**.
>
> A QR code often contains the postcode with unusual spacing or extra
> characters. Sometimes the wrong postcode is printed, and one postcode can
> cover more than one walk.
>
> Some labels also have a **1D barcode** at the bottom right containing the
> postcode. You can scan these as well.

### 2. Battery moved to the centre

It now sits midway between the ⚙️ gear and the RESET button, with a spacer on
each side, so it stays centred whatever the bar width.

### 3. Bottom bar no longer blue

The blue background is gone — it's plain text on the page background now. The
text colour changed from white to the app's normal grey, or it would have been
invisible. Tapping it still takes you home.

### 4. The two type-in buttons

More space between them (8px → 16px gap) and bolder text (15.5px/800 →
16.5px/900).

---

## Checks run

- t51.cjs: 23/23, including a real scan-then-tap-the-title journey proving HOME
  still works after the bottom bar restyle. Build 165 fails exactly the ten
  changed checks and passes the rest.
- 250-label render diff vs build 165: **0 differences.** The resolver and all
  result rendering are untouched.
- All six script blocks syntax-checked.
- The app's own update sanity gate accepts this file; the gate function is
  byte-identical to build 165.
- 14 changed lines.
- Stamps: THIS_BUILD_NUM 166, REPO_APK_BUILD 164, BAKED_DATA_BUILD stamped down
  to 57 so older APKs still accept it. Data untouched (build 131, a5195d2e,
  24,102 rows) — no `wild_data.json` upload needed.

### Near miss worth recording

The "make the buttons bolder" edit was originally written as a plain
find-and-replace on the style string. That string matched **three** buttons —
the two type-in buttons and the 🗺️ map-link button inside the results template
— plus two more times inside JavaScript template literals. An assertion caught
it before anything was built. The edit is now keyed on each button's own id,
with a check that exactly two buttons changed and the map link did not.

This is the second time this exact shape has bitten (the Recent "Back" relabel
matched 12 unrelated buttons). Style strings in this file are not unique — key
edits on ids, and always assert the match count.
