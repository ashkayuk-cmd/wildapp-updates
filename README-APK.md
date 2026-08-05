# Wild App — Android APK

## ⚠️ BEFORE INSTALLING ANY UPDATE (v1.7+)
Kiosk mode fights the installer.
**Hold BACK for 10 seconds → "Leave" → then install the new APK.**

## ⚠️ HOW TO GET OUT / UNDO EVERYTHING (read once, remember it)
1. Hold **BACK for 10 seconds** → the dialog appears.
2. Tap **"Android Settings"** — this always works, even when Wild App is
   the Home app and there's no app drawer.
3. To stop Wild App being the Home app:
   Settings → **Apps** → (gear/menu) **Default apps** → **Home app** →
   pick your original launcher.
   (Some builds: Settings → **Home** → choose launcher.)

## v1.48 (current — WildApp.apk)  · versionCode 44 · DATA UPDATE

Rebuilt from the `wild_data.xlsx` you sent, and it also carries the v1.47 fix
(the ⟳ Update button no longer sits under the red RESET button), so this one
APK brings you fully up to date.

**24,147 addresses — what changed from what was on the PDA:**

| | |
|---|---|
| Leinster House 44-46, Leinster Gardens | 20 HALLFIELD → **35 CLEVELAND** |
| Hilton London Paddington, Eastbourne Terrace | FIRM → **12 WESTCLIFFE** (the "1-2" naming tidied) |
| 225 Edgware Road | Contrasti salon row is now **Hilton London Metropole**, FIRM |
| Oak Tree Lodge 49-50, Leinster Gardens (21 flats) | 35 CLEVELAND → **20 HALLFIELD** |

That last row is the one to check — see below.

**Oak Tree Lodge:** your sheet has these 21 flats on 20 HALLFIELD, but they
were moved to 35 CLEVELAND back in v1.39. Built as your sheet has it. Scanning
still works cleanly either way — a label naming Oak Tree Lodge gives
20 HALLFIELD, and 43 / 47-48 / Leinster House give 35 CLEVELAND. Only a label
with the bare postcode and no address text will now ask which walk, because
W2 3AT covers both. Say the word and I'll flip them back.

**Install:** same key, installs over the top. Hold **BACK 10 seconds →
"Leave"** first, then upload this `WildApp.apk` to the repo and set
`version.txt` to **`44`**.

---

## v1.47 — OVER-THE-AIR UPDATE, NO APK  · content build 43

**First update delivered without an APK.** Put `index.html` and `content.json`
in `ashkayuk-cmd/wildapp-updates`, then tap **⟳ Update** on the PDA. Nothing
to install; the PDA is still running the build 42 APK underneath.

**The fix:** the red **RESET** button is `position:fixed` in the top-right
corner, so it floats OVER the title bar and was sitting on top of the
⟳ Update button. The bar now measures RESET and reserves exactly that much
room on its right, so nothing ends up underneath it (the title shortens with
an ellipsis if space runs short). Because it measures rather than guesses, it
stays correct if the RESET label or the screen size changes.

**Two more self-rejection bugs found while testing this** — both would have
stopped over-the-air updates working at all:

- The check that a downloaded file "really is the app" looked for
  `var BUILD_NUM=<number>`, a stamp v1.45 replaced with a derived value — so
  every published update would have been refused. It now looks for the
  constant the app actually carries.
- (v1.46 fixed the same class of bug in the anti-loop marker.)

Found by a new end-to-end test that publishes this exact file to a mock repo,
lets the update layer download and cache it, then hands that cache to the real
loader and checks it boots this build — the whole path, start to finish.

**To publish:** upload `index.html` and `content.json` to the repo (replacing
`content.json`), then tap ⟳ Update → "Update ready" → tap the green banner.
No `version.txt` change and no install — that's only for APK builds.

---

## v1.46 (WildApp.apk)  ⚠ replaces v1.45 · versionCode 42

Everything in v1.45, plus a **⟳ Update** button in the top bar, next to
"Handheld Scanner". Tap it any time to check right now instead of waiting for
the automatic check. It tells you what happened:

- **Up to date** — nothing new
- **Update ready** — something was downloaded; the green banner restarts the
  app to apply it
- **New version** — a new APK is out (that one still needs installing)
- **No signal** — couldn't reach GitHub, so it isn't claiming anything

The same wording now appears on **Check now** in Manage corrections → App
version.

Also fixed while adding it: an update that had been downloaded but not yet
restarted into was counted as already running, so a second check could say
"up to date" while the update was still waiting.

**If you haven't installed v1.45 yet, install this one instead — it replaces
it.** Same key, installs over the top. Hold **BACK 10 seconds → "Leave"**
first, then upload this `WildApp.apk` to the repo and set `version.txt` to
**`42`** (and `content.json` → `{"code": 42, "data": 42}`).

---

## v1.45 (WildApp.apk)  ⚠ built on v1.44 · versionCode 41

**Automatic updates.** From this build on, most updates arrive on their own —
no APK, no installer, no leaving the kiosk.

**1. It checks properly now.** The old check ran once at launch, on a PDA that
never restarts. It now checks at startup, every three hours, and whenever the
screen wakes after half an hour or more.

**2. The app and the address data update over the air.** Both are fetched from
the same GitHub repo as the APK, cached on the device (compressed, works
offline afterwards) and used from the next restart. The compiled part of the
app hasn't changed since build 37 — v1.42, v1.43 and v1.44 were all app-file
changes — so from now on updates like those will just appear.

Nothing is ever swapped in mid-round. A downloaded update sits in the cache
until you tap the banner or the app is next started.

**3. If an update ever goes wrong**, it can't strand you:

- A downloaded copy has to pass a sanity check before it's kept, and again
  before it's booted (right size, really this app, not a 404 page).
- A flag is set before a downloaded copy starts and cleared once it has
  started. If one ever fails to start, the next launch throws it away and
  falls back to the version inside the APK.
- **Manage corrections → App version → "Use built-in version"** does the same
  thing on demand, and there's a **Check now** button next to it. Your
  corrections and scan history are never touched by any of this.

The version tag at the bottom of the screen shows which build is actually
running, marked *(live)* when it's a downloaded one.

### How to publish an update from now on

For an **app or data update** (no install for you):

1. Put the new `index.html` and/or `wild_data.json` in
   `ashkayuk-cmd/wildapp-updates`.
2. Edit `content.json` there:
   `{"code": 42, "data": 41}` — raise `code` for a new app file, `data` for
   new addresses.

That's it. The PDA picks it up within a few hours, or straight away with
**Check now**.

For a **native change** (rare — the wrapper, the scanner, the BACK key), it's
still an APK: upload `WildApp.apk` and set `version.txt`, same as always.

### Inside the APK

`assets/index.html` is now a small loader (4 KB); the app itself moved to
`assets/app.html`. The loader is deliberately tiny and dependency-free — it's
the piece that has to keep working. The native code is untouched and
byte-identical.

**Install:** same key, installs over the top. Hold **BACK 10 seconds →
"Leave"** first.

**To publish this one:** upload `WildApp.apk` and set `version.txt` to **`41`**.

---

## v1.44 (WildApp.apk)  ⚠ built on v1.43 · versionCode 40

**Fixes the "SOUTH CONCIERGE" scan showing walk 6 instead of walk 29.**

*What was wrong:* the label carries postcode **W2 4BF** (139 Queensway — all
36 addresses there are on **29 HATHERLEY**) and the text "SOUTH CONCIERGE".
The round has exactly one address containing the word CONCIERGE —
*Concierge 6, Hermitage Street, W2 1BE*, on **6 SHELDON**. Because that word
appears on only one street, the matcher's confidence floor rated it a
"confident" match, and the postcode/address cross-check added in v1.31 then
decided the printed postcode must be wrong and handed the walk to the address.
One incidental word was outranking a perfectly good postcode. That's why it
was right on older builds — the cross-check didn't exist before v1.31.

*The fix:* the address text now has to be **corroborated** before it can beat
a printed postcode:

- at least **two** of the matched address's distinctive words appear on the
  label (or one, plus a house number that address actually has), **and**
- at least one of those words is genuinely distinctive — STREET, HOUSE,
  COURT and the like prove nothing, **and**
- none of the **scanned postcode's own addresses** explain the same text just
  as well — if they do, the postcode wins, because it's the harder evidence.

A label that really does carry the wrong postcode (full address text, two or
more distinctive words, matching number) still gets the walk from the address
with the amber warning, exactly as before. Everything else falls through to
the normal postcode path.

Checked on the real data: your scan now gives **29 HATHERLEY**; six other
one-word labels aimed at a foreign postcode all stay on the scanned postcode;
a genuine wrong-postcode label still diverts; and 65 Alfred Road → FIRM,
31 Westbourne Gardens → 22 PORCHESTER and the Harbour Club all still resolve
as before.

Data unchanged (24,147 records) and the compiled code is byte-identical to
v1.41–v1.43, so the hardware BACK key behaves exactly as before.

**Install:** same key, installs over the top. Hold **BACK 10 seconds →
"Leave"** first.

**To publish the OTA update (both steps):** upload this `WildApp.apk` to
`ashkayuk-cmd/wildapp-updates` **and** set `version.txt` to **`40`**.

---

## v1.43 (WildApp.apk)  ⚠ built on v1.42 · versionCode 39

Two fixes to the correction flow, plus one bug found along the way.

**1. BACK works on the "Saved" screen.**
That confirmation page was the only screen in the app that cleared its
"a sub-screen is open" marker, so the BACK key had nothing to close and did
nothing. It's now a normal sub-screen: BACK returns to the scan — which
re-runs it, so you land on the corrected result. There's also a
**← Back to the scan** button and a *Manage my corrections* link on it.

Same dead end is fixed after picking a walk off a **Possible walks** list:
that result now carries the raw block and the fix button like any other, and
BACK goes back to the list.

**2. More options when you change a walk.**
- The **"Apply … to"** chooser now appears **every time**. Before, a label
  with a house number saved instantly with no choice at all. Options:
  **Just this address** · **Every "5" in W2 1LZ** (when the label has a
  number) · **Every address in W2 1LZ**.
- New **address & postcode editor** — 📍 *Change the address or postcode
  too…*. Type the right address and postcode, or tap one of the real
  addresses the round already knows for that postcode. Also reachable
  without touching the walk, via *📍 Address or postcode wrong? Fix that
  instead* on the walk picker.
- The fix stays keyed on the postcode the **label** carries (that's what the
  next label will have), while showing the postcode you entered. The
  corrections list and the backup text now show both.

**3. Bug found while in there:** "Just this address" was keyed on the whole
barcode payload — which includes the per-item tracking number. So a fix only
ever matched *that one parcel*; the next parcel to the same door looked like
a new address and the correction silently stopped applying. It now keys on an
address fingerprint (address letters with all spacing stripped, plus the
number and the Mailmark DPS; tracking numbers, postcodes and item IDs
removed), so a door fixed once stays fixed. Corrections saved by v1.39–v1.42
are still read, so nothing already saved is lost.

Data unchanged (24,147 records, byte-identical to v1.42), and the compiled
code is byte-identical to v1.41/v1.42, so the hardware BACK fix behaves
exactly as before.

**Install:** same key, installs over the top. Hold **BACK 10 seconds →
"Leave"** first.

**To publish the OTA update (both steps):** upload this `WildApp.apk` to
`ashkayuk-cmd/wildapp-updates` **and** set `version.txt` to **`39`**.

---

## v1.42 (WildApp.apk)  ⚠ built on v1.41 · versionCode 38

Data update — three corrections, keeping the working hardware-BACK fix from v1.41 (the app's compiled code is byte-identical to v1.41, so BACK behaves exactly as before).

- **43 Leinster Gardens (W2 3AT)** → walk **35 CLEVELAND** (was 20 HALLFIELD)
- **47-48 Leinster Gardens (W2 3AT)** → walk **35 CLEVELAND** (was 20 HALLFIELD)
- **Paddington Exchange, Hermitage Street (W2 1HG)** — fixed the street spelling "Hermitage Steet" → "Hermitage Street" (walk unchanged, still 6 SHELDON)

All of W2 3AT / Leinster Gardens now sits on 35 CLEVELAND (24 addresses); none left on 20 HALLFIELD. Still 24,147 records.

I also added the Paddington Exchange row to the master `wild_data.xlsx` (it was missing from the embedded copy, which is why the spelling only existed in the app's baked data), so the spreadsheet and the app now match.

**Install:** same key, installs over the top. Hold **BACK 10 seconds → "Leave"** first.

**To publish the OTA update (both steps):** upload this `WildApp.apk` to `ashkayuk-cmd/wildapp-updates` **and** set `version.txt` to **`38`**.

---

## v1.41 (current — WildApp.apk)  ⚠ built on v1.40 · versionCode 37

**The Android hardware BACK button now works inside the app.** This is a
native fix, not a web one.

*What was wrong:* the kiosk was built to completely swallow the BACK key (so
the app stays pinned as the Home screen). A short BACK press did nothing but
cancel the 10-second-hold-to-exit timer — it never reached the in-app back
handler, which is why the earlier attempt (a web-page back handler) never
fired: the key press wasn’t being handed to the web page at all.

*The fix:* a quick tap of BACK is now forwarded into the app, so it closes
whatever is open — the Wrong-walk pickers, the scope chooser, Manage / Recent
/ backup / GitHub screens, the info/voice/lens sheets, the address detail
sheet, the camera panel, or an expanded walk row — one step at a time. The
**10-second hold → “Leave”** escape is unchanged and still works exactly as
before (a long hold still reaches the exit prompt; only a short tap now does
the in-app back).

No data or address changes — same 24,147 records as v1.40.

**Install:** same signing key, version number up to 37, installs over the top.
Remember the **10-second-BACK → “Leave”** step first (kiosk fights the installer).

**To publish the OTA update (both steps):** upload this `WildApp.apk` to the
`ashkayuk-cmd/wildapp-updates` repo **and** set `version.txt` to **`37`**.

---

## v1.40 (current — WildApp.apk)  ⚠ built on v1.39 · versionCode 36

**Data update only** — no app changes. Same code as v1.39 (the walk-scoping
correction and the universal BACK button are all still there), just refreshed
addresses. Installs straight over the current app (same signing key), so your
saved corrections and scan history are kept.

- **L H A London Ltd (Leinster House, 44-46 Leinster Gardens, W2 3AT)** moved
  from **20 HALLFIELD → 35 CLEVELAND**. 24,147 records, everything else
  unchanged.
  *(Two other W2 3AT addresses — 43 and 47-48 Leinster Gardens — are still on
  20 HALLFIELD; I left those as they were. Say the word if they should move
  too.)*
  *(The earlier “Hermitage Steet” typo on the Paddington Exchange row is still
  there — harmless for scanning, only the Walks-tab street header shows it.
  Tell me and I’ll fix it.)*

**Install:** same signing key, version number up to 36, installs over the top.
Remember the **10-second-BACK → “Leave”** step first (kiosk fights the installer).

**To publish the OTA update (both steps):** upload this `WildApp.apk` to the
`ashkayuk-cmd/wildapp-updates` repo **and** set `version.txt` to **`36`**.

---

## v1.39 (current — WildApp.apk)  ⚠ built on v1.38 · versionCode 35

Two app changes plus a data update. Installs straight over the current app
(same signing key), so your saved corrections and scan history are kept.

- **Correcting a walk no longer changes the whole postcode by accident.**
  When you tap “Wrong walk?” on a scan that has **no house number**, the app
  now asks what to change: **“Just this address”** or **“Every address in
  \<postcode\>”**. Before, a numberless correction quietly re-walked *every*
  numberless scan of that postcode. Scans that already have a house number are
  pinned to that number as before. The **Manage my corrections** screen now
  labels each saved fix — THIS ADDRESS ONLY / WHOLE POSTCODE / THIS HOUSE
  NUMBER — so you can see (and delete) exactly what each one covers.

- **The BACK button now works on every screen.** Pressing BACK closes whatever
  is open — the Wrong-walk pickers, the scope chooser, Manage / Recent /
  backup / GitHub screens, the scanner info/voice/lens sheets, the address
  detail sheet, the camera panel, or an expanded walk row — one step at a time.
  It never closes the app itself (the 10-second-BACK → “Leave” escape at the
  top of this file is still how you get out).

- **Data updated.** Oak Tree Lodge (49-50 Leinster Gardens, W2 3AT, 21 flats)
  moved from **20 HALLFIELD → 35 CLEVELAND**, and a new **Paddington Exchange**
  (Hermitage Street, W2 1HG) row was added on **6 SHELDON**. 24,147 records.
  *(Heads-up: that new row’s street column is spelled “Hermitage Steet” —
  scanning by the W2 1HG postcode works fine; only the Walks-tab street header
  would show the typo. Say the word and I’ll correct it.)*

**Install:** same signing key as every build since v1.7, version number goes
up (35), so it installs straight over the current app. Remember the
**10-second-BACK → “Leave”** step first (kiosk mode fights the installer).

**To publish the OTA update (both steps):** in the `ashkayuk-cmd/wildapp-updates`
repo, upload this `WildApp.apk` **and** set `version.txt` to **`35`** (plain
number). If you bump the APK but leave version.txt behind, the phone never
shows the update banner.

---

## v1.38 (current — WildApp.apk)  ⚠ built on v1.37 · versionCode 34

Three scan fixes. **Data unchanged** from the last few builds (same
24,146-record dataset, same wild_data.json/xlsx) — only the app logic
(index.html), the manifest version number, and the signatures changed.

- **Name / worker-code scans no longer guess a firm.** Scanning a 1D code
  that's just a dotted person-name (e.g. `susan.d.williams`, `fran.llorente`)
  used to fuzzy-match a real surname and confidently show ONE firm/walk —
  which was wrong (`susan.d.williams` → "William Hill Bookmakers 74 Queensway,
  walk 24" made no sense; `fran.llorente` → "Knight Frank" was a lucky guess
  from the *same* broken mechanism). These codes now show a plain
  **"🪪 Name / worker code — no delivery address"** card (or register as
  "no postcode" on a live trigger-scan) instead of inventing a walk.
  *Trade-off, being honest:* this also stops the occasional right-ish guess
  like fran.llorente→Knight Frank. There's no reliable way to keep the good
  guesses without the bad ones — the app can't tell a real surname from a
  coincidence. If you actually want name→firm lookup, that's a separate
  feature (a real staff/firm name list), not a fuzzy guess — say the word.

- **Junction Mews "12" vs "1-2" now asks instead of assuming.** A barcode for
  **1-2 Junction Mews** (your firm) often arrives glued as `12` because
  Mailmark drops the hyphen — identical to a genuine **No. 12 Junction Mews**.
  The app can't tell them apart from the digits alone, so a `12 … Junction
  Mews` scan (W2 1PN) now shows a **two-button picker**:
  **① 1-2 Junction Mews (firm)** or **② 12 Junction Mews → walk 11 Norfolk**,
  with a note explaining why. Tap the right one. A scan that keeps the hyphen
  (`1-2` / `1 2`) still snaps straight to the firm as before — only the glued
  `12` asks.
  *(Note: the dataset still lists "1-2, Junction Mews" as walk 11 while your
  hand-override says it's a firm — I did NOT change any walk assignments.
  Flagging it so you can decide; nothing was auto-reassigned.)*

**Install:** same signing key as every build since v1.7, version number goes
up (34), so it installs straight over the current app. Remember the
**10-second-BACK → "Leave"** step first (kiosk mode fights the installer).

**To publish the OTA update (both steps — this is the bit that's easy to
half-do):** in the `ashkayuk-cmd/wildapp-updates` repo, upload this
`WildApp.apk` **and** set `version.txt` to **`34`** (plain number, nothing
else). If you bump the APK but leave version.txt behind, the phone never
shows the update banner.

---

## v1.31 (older)  ⚠ built on v1.30
Four changes plus the dataset update.

- **Sleeps after 8 minutes idle.** With no tap, key, or trigger for 8 minutes
  the screen dims to near-black to save battery on the round. **Any** touch,
  key, or a trigger-pull wakes it instantly — and a trigger-pull both wakes
  the screen *and* scans in one pull, so there's nothing extra to clear. The
  camera keeps running underneath, so waking is immediate (no camera restart).

- **Recents button / back button.** Recents is already blocked and the app is
  the Home app (set up back in v1.8), so while Wild App is running there's no
  recents to swipe it away and BACK does nothing — that behaviour is
  unchanged and needed no rebuild. (If you ever need OUT, the 10-second-BACK
  escape at the top of this file still works.)

- **Tracking / bare barcodes are ignored — everywhere now.** Previously only
  the exact Royal Mail S10 shape (`YR581784168GB`) was skipped, and only on
  the Handheld screen. Now **any** live scan that carries no postcode *and* no
  matchable address — a courier tracking number, a bare 1D item barcode, a
  meaningless code — is ignored completely on both the handheld and the camera
  scanner: no beep, no buzz, no history entry, and whatever's already on
  screen stays put. Just aim at the address code; there's no interruption to
  clear. Real addresses still scan normally, **including a plain building-name
  QR with no postcode** (that still resolves, so it's never skipped). Typing a
  code in by hand still looks it up — the ignore only applies to live scans.

- **Postcode ↔ address cross-check.** When a QR carries **both** an address
  and a postcode and they don't agree, the app now trusts the **address** for
  the walk (the reliable locator) and shows an amber **"⚠ Postcode doesn't
  match this address"** banner that also tells you the postcode the address
  *should* have. E.g. an address that's really W2 1LZ printed with W2 6AA on
  it → you still get the right walk, with a clear warning and "this address is
  W2 1LZ". When the postcode and address agree (the normal case) there's no
  warning. The scan history records the mismatch too.

- **Dataset updated** (`wild_data.xlsx` re-baked in): **31 Westbourne Gardens**
  postcode corrected **W2 5PX → W2 5NR** (walk 22 PORCHESTER unchanged).
  24,155 addresses, everything else identical.

Installs straight over v1.30 (same signing key, higher version number = 27) —
your saved corrections and scan history are kept. Remember the
**10-second-BACK → "Leave"** step above before installing.

**Heads-up — like v1.30, this build does NOT include the v1.29 "upload
corrections to GitHub" feature** (both were built on the v1.28/v1.30 line, not
on v1.29). If you want the tracking-ignore + sleep + mismatch-warning changes
*and* the corrections-upload feature in one APK, say so and I'll merge them.

**To publish as an OTA update:** upload this `WildApp.apk` to the
`wildapp-updates` repo as `WildApp.apk` and set `version.txt` to **27**. Any
PDA on an older build shows the "⬇ Update available (build 27)" banner.

## v1.30 (previous)  ⚠ built on v1.28
The handheld now **ignores item tracking barcodes**. When a live trigger scan
reads a Royal Mail / S10 tracking number — the two-letters, nine-digits,
two-letters shape like `YR581784168GB`, with no address on it — the scanner
does **nothing at all**: no beep, no buzz, no history entry, and whatever
result is already on screen stays put. Just aim again at the address barcode;
there's no interruption to clear. Only that exact tracking shape is skipped, so
every real address still scans normally — including a plain building-name QR
with no postcode. (If you open such a code from Recent scans by tapping it, it
shows a short "Tracking number" note instead, so you're not left staring at a
blank screen.)

**Heads-up — this build does NOT include the v1.29 "upload corrections to
GitHub" feature.** It was built on top of the **v1.28** APK you sent, not the
v1.29 built earlier. If you want both the tracking-ignore behaviour *and* the
corrections-upload feature in one APK, say so and I'll merge them into a single
build.

Installs straight over v1.28 (same signing key, higher version number = 26) —
your saved corrections and scan history are kept. Remember the
**10-second-BACK → "Leave"** step above before installing.

## v1.28 (WildApp-v1.28-kiosk.apk)
Turns ON the over-the-air update check (built dormant back in v1.25). On
launch, if the PDA is online, the app checks your GitHub repo
(`ashkayuk-cmd/wildapp-updates`) and shows a "⬇ Update available — tap to
install" banner when a newer build has been posted. Offline-safe: no
connection = no check, and scanning is never affected.

**Publishing a future update (the routine from now on):**
1. Claude builds the new signed APK and sends it to you.
2. In the `wildapp-updates` repo: upload it as **WildApp.apk** (replacing the
   old one), and edit **version.txt** to the new build number.
3. Any PDA still on an older build shows the banner on its next launch.

`version.txt` holds the build number of the latest APK in the repo (now 24);
the banner appears only when that number is higher than the build installed on
the device. The one thing still to confirm on the device is that tapping the
banner opens Android's installer (see the note when you first test it).

Installs over v1.27 (same key) — your corrections and scan history are kept.

## v1.27 (WildApp-v1.27-kiosk.apk)
Two additions to the handheld screen, both aimed at trusting and fixing scans.
Neither touches the address matcher — resolution behaves exactly as in v1.26.

- **Recent scans** — a new link under any scanned result. Shows your last
  scans, colour-coded down the left edge: your own correction (green), a
  clean single walk (blue), more than one possible walk (amber), or one that
  needs attention — out of area / no postcode / no match (red). Tap any scan
  to see it again; from there the **"Wrong walk?"** fix is one tap away. It
  only appears under a result, so the idle scanning screen stays clean.

- **Back up / copy all corrections** — on "Manage my corrections", a button
  that lists every fix you've made as plain text you can copy. Keep it as a
  backup, or send it on so those fixes can be built into the app permanently.
  Worth doing occasionally: a clean reinstall clears on-device corrections.

Installs straight over v1.26 (same key, higher version number) — your saved
corrections and scan history are kept. Remember the 10-second-BACK → "Leave"
step above before installing.

## v1.8
Closes the HOME and RECENTS holes from v1.7:
- **HOME key neutralised** — the app can now be set as the device's Home
  app, so pressing HOME simply re-shows Wild App. **This needs one setup
  step (below) or HOME will still exit.**
- **RECENTS**: the app no longer appears in the recents list (so it can't
  be swiped away to kill it), and opening recents or pulling down the
  notification shade now collapses again straight away.
- **Settings escape** added to the 10-second-hold dialog (see above).

### Setup step — do this after installing
Press the **HOME** key once. Android asks which Home app to use → choose
**Wild App**, and tick **Always / Use by default**.
If no prompt appears: Settings → Apps → Default apps → Home app → Wild App.

After that: HOME re-shows the scanner, RECENTS closes itself, BACK does
nothing, and the PDA boots straight into the app.

### What still can't be blocked (honest)
**Force stop** via Settings → Apps → Wild App → Force stop cannot be
prevented by any ordinary app — only a device-management enrolment can
restrict Settings itself. The consolation: because Wild App is the Home
app, pressing HOME after a force stop relaunches it immediately.

For a genuinely sealed device, use Zebra's **Enterprise Home Screen**
(free, installs like this APK, no PC needed) — it replaces the launcher,
hides Settings, and can be password-protected. It works alongside this app.

## v1.7
Auto-start on boot; kiosk mode with the 10-second BACK escape; Maps button
removed. (HOME/RECENTS still worked — fixed in v1.8.)

## v1.6
All header buttons hard-removed, voice deleted, scan result permanently on,
walk name auto-fits one line, database pre-baked (fast launch),
OCR/camera libs stripped (23 MB → under 1 MB).

## v1.5 — Handheld-only build
Boots straight to the Handheld scanner; scans delivered as one direct
call (major per-scan speedup); speech engine not started.

## v1.4
Trigger fix (EMDK permission — cured "bind refused"), Chrome-parity
viewport handling (cured the stuck scrolling), engine-version launch
message, no title bar.

## v1.3
Scrolling fallback for pre-108 engines (dvh → vh), keyboard resizes the
page instead of covering it, diagnostic panel only on true startup failure
and now closable.

## v1.2
Adds **yellow-trigger scanning on Zebra devices**. The app binds to the
scanner service built into the device itself (EMDK — no DataWedge needed).
On launch on a Zebra device you should see a small message:
"Zebra scanner ready — pull the trigger". Trigger scans are fed into the
app through the same pipeline as a plug-in wedge scanner: the scanner
panel opens by itself, and tones/speech/history all work. On a normal
phone this feature stays dormant and the camera scanner is unchanged.
If a different message appears at launch (a bind/init error), report its
exact wording — that pinpoints the fix for the next build.

## v1.1
Same app plus a boot watchdog: if the app can't start, a red panel
appears after ~4 seconds showing the WebView engine version and the first
errors caught. Read that panel back when reporting a problem. It stays
invisible when the app starts normally. Installs straight over v1.0
(same signing key, higher version number) — settings are kept.

## Engine requirement (why v1.0 sat dead on the TC56)
The app's JavaScript needs a **Chrome 80+ WebView engine**. Factory
Android 7 devices ship with a ~Chrome 51–58 engine, on which the script
can't even parse — the screen paints but nothing responds. Fix: update
**Chrome** and **Android System WebView** in the Play Store, then reopen.
The Play Store automatically installs the newest version that still
supports Android 7 (up to roughly Chrome 119) — far more than enough.

A self-contained, installable Android app: your index.html + wild_data.xlsx +
all scanner/OCR libraries baked in. Works with no signal at all.

- Package: uk.wild.app · minimum Android 5.0, tested target Android 7 (TC56)
- Signed v1+v2+v3, so it installs on the TC56 AND on any modern phone.

## Install (on the TC56 or any Android phone)

1. Copy `WildApp.apk` onto the device (download it in the device's browser,
   or transfer via USB / cloud drive).
2. Settings → Security → **Unknown sources** ON (you've already done this
   on the TC56).
3. Open the APK in the Files app → Install. If Play Protect complains about
   an unknown developer, choose "Install anyway" — it's self-signed, which
   is expected.
4. First launch: allow the **Camera** permission when asked.

## What works

- The whole app, offline: walks, search, WildMoves, camera QR/Mailmark
  scanning, TEXT SCAN (OCR engine + English data are bundled), speech
  (SAY WALK now speaks through Android's own text-to-speech — your speed
  and pitch sliders still apply), Maps buttons open the Google Maps app.

## Honest limitations of the wrapper

- **Hardware trigger**: this APK uses the camera scanner. The TC56's yellow
  trigger only feeds apps through DataWedge (missing on your unit) — the
  Enterprise Browser package remains the trigger-based route.
- **Camera decode speed**: inside a WebView there's no native BarcodeDetector,
  so scanning runs on the ZXing path — works, a touch slower than Chrome.
- **Exports/downloads** ("Export Excel database" etc.) don't save files from
  inside the wrapper. Do your editing/exporting in Chrome or the website as
  you do now; the APK is for scanning and lookups on the round.
- **Updating the app or dataset** means building a new APK (the files are
  baked in). Ask Claude for a rebuild with your new index.html/xlsx.

## Keep the keystore

`wild-signing.keystore` (alias `wild`, password `wildapp2026`) signed this
APK. Android only installs an UPDATE over an existing app if it's signed
with the SAME key — so keep this file safe and provide it when asking for
a rebuilt/updated APK. Losing it means uninstalling before installing any
future version (which wipes the app's saved settings/history).
