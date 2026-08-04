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
