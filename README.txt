WILD APP — v1.80 (build 76)
Over-the-air code update. ONE FILE. No APK, no install, no data file.

-------------------------------------------------------------------
HOW TO RELEASE
-------------------------------------------------------------------
1. Upload index.html to ashkayuk-cmd/wildapp-updates (main), replacing
   the existing index.html. Nothing else changes — do NOT touch
   wild_data.json, WildApp.apk or content.json.
2. On the PDA, tap the "⟳ Update" button (bottom left).
   It turns green: "Update ready".
3. Tap it again to apply. The app reloads on build 76.

Stamps in this file:
  build          76   ("v1.80")   — beats the published 75 and the APK's 74
  data build     72   hash e71e95e7   rows 24149   (UNCHANGED)
  BAKED_DATA_BUILD 57  (standing rule for OTA files)
  REPO_APK_BUILD 74   — corrected; the published file said 72 while the
                        APK actually sitting in your repo is build 74.
                        Harmless either way, but it was wrong.

-------------------------------------------------------------------
WHAT THIS FIXES
-------------------------------------------------------------------
The label you scanned:

  JGB 6209G24C0737792000114156963450 1040826040826ITL01 IT828318780GB
  47 49 WESTBOURNE GROVE W24JZ 9ZGB UB78JD 46241 PG18048178548202

The sender printed W2 4JZ. In your data that postcode is 148 addresses
at Lancaster Close / St. Petersburgh Place, all walk 25 — so the app
answered 25. The real address, 47-49 Westbourne Grove, is W2 4UA and
walk 28 KGS.

The app already had a guard against a wrong printed postcode, but it
only fired when the label carried a RARE word — one your round confines
to a street or two (CITI, TALBOT). This label has none: WESTBOURNE is on
18 streets in your round, GROVE on 4. So the guard was blind to the
commonest label shape of all — a plain "<number> <street>" line.

NEW RULE (streetNumberOnLabel): the evidence is the PAIR — the whole
street name as printed, plus a number that exists on that street and
nowhere at the scanned postcode. Every condition below is a veto, and
the fallback is exactly the old behaviour:

  1. the WHOLE street name must appear in the label with whitespace
     squashed out (the Zebra breaks words up mid-word), and a street
     name contained inside another matched one is not evidence — so
     "Bourne Terrace" can never win on a label reading WESTBOURNE
     TERRACE. There are 28 such pairs in your data. Exactly one street
     may survive;
  2. the scanned postcode must cover NONE of that street;
  3. the number must not exist at the scanned postcode either;
  4. the street + number must land on exactly one walk and exactly one
     postcode, and not the scanned one.

The number is read the way an address line reads: the digits printed
immediately BEFORE the street name. So "FLAT 55 47 49 WESTBOURNE GROVE"
uses the block 47-49, not the flat — and the flat is still pinned on
screen afterwards. Only the delivery half of a Mailmark payload is read;
the sender's own details after the ZGB marker are never taken for the
delivery address.

ON SCREEN NOW
  Walk 28 KGS
  47-49, Westbourne Grove, W2 4UA
  ⚠ Postcode doesn't match this address
     Scanned W2 4JZ — but this address is W2 4UA. Walk taken from the
     address.
  ⚠ Or, by the label's postcode · W2 4JZ →  25 ST PETERSBURGH  (red,
     tappable, in case it ever is the right one)

-------------------------------------------------------------------
TESTING
-------------------------------------------------------------------
Everything below ran against this exact file, in a real browser engine,
booting the whole app the way the PDA does.

  ALL 24,149 records labelled with their OWN (correct) postcode:
      the new rule fired 0 times. It cannot touch a good label.

  ALL 24,149 records labelled with someone else's postcode:
      11,058 rescued — every one to the right walk, 0 wrong.
      (The rest fall through to the old behaviour, unchanged.)

  250-label render diff against your running build 75, including the
  W2 1PN firm case, the Talbot Square label, a bare tracking code, a
  mid-word-space label and the concierge case:  0 differences.

  26 unit tests: the reported label, each of the four gates, the
  sender-half guard, the flat-inside-a-block case, untouched behaviour,
  and every gate the running app applies before accepting this file.

ONE THING THE SWEEP FOUND
Hereford Road holds both "2 5, Hereford Road" (walk 33) and "Flat 2-5,
Berrington House" (walk 28). The record that should have blocked a guess
was invisible, because the app's range normaliser joins "2 5" into "2-5"
on a LABEL but leaves it alone inside a record's own text. Both
spellings are now checked. That killed the single wrong answer in the
first sweep and raised the rescue rate at the same time.

-------------------------------------------------------------------
SCOPE
-------------------------------------------------------------------
The change is in the handheld path — the one your PDA's trigger uses.
The phone camera path is untouched.

Still open from before: the GitHub token stays unbaked until the PAT is
narrowed to Issues:RW / Contents:No access.
