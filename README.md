# Wild App — APK v1.93 (app build v4.63 / 178)

## What to upload to `ashkayuk-cmd/wildapp-updates`

| File | Needed? |
|---|---|
| `WildApp.apk` | **Yes** — install on the TC56 |
| `index.html` | **Yes** — this is the OTA copy of the same web build |
| `wild_data.json` | No — dataset unchanged (24,102 rows, hash `a5195d2e`) |

Install the APK first, then upload both files so the repo and the phone agree.
Signed with your existing key (SHA-256 `2C:3A:BB:7A…`), verified against the
signature on the currently-installed APK — it installs straight over the top.
**Nothing is wiped**: corrections, Today's Walk and settings all survive.

---

## 1. The double scan sound — fixed (native)

The shipped APK **never configured the scanner at all**. `ZebraScanner` went
`getDevice → addDataListener → enable → read`, with no `ScannerConfig`
anywhere, so Zebra's own decode beep ran at its factory default (on) and the
app's outcome tone played on top of it. Two sounds every scan, and the engine
one ignored your Sound slider because it was never part of the page.

New private method `silenceEngineFeedback()`:
- blanks `scannerParams.decodeAudioFeedbackUri` (EMDK's own way of saying "no sound")
- clears `scannerParams.decodeHapticFeedback`, so the app's vibration switch is
  the only thing that buzzes
- called between `enable()` and `read()` in **both** `acquireScanner()` and
  `resume()` — setConfig needs the scanner enabled and has to run before the
  read, and re-enabling after a pause can hand back the defaults
- wrapped in its own try/catch: a firmware missing either field loses the
  silencing, not the scanner

Now one sound per scan, and it's the meaningful one.

## 2. A double *buzz* found on the way (web)

Three result paths — your correction, an override, and the Junction Mews
picker — fired their own 40 ms buzz and then called `outcomeTone()`, which
buzzes again. Those pre-buzzes are gone; `outcomeTone()` owns the haptic, as
its own comments always said it did.

## 3. Sound & vibration (web)

The Sound screen rebuilt in the shape of Android's own Sound page:

- **Beep volume** — as before
- **Vibrate on scan** — master switch. Off, with volume at 0, makes the app silent.
- **Strength** — Light / Normal / Strong
- **Test each outcome** — a row per tone (exact, building, more-than-one-walk,
  no match, not your walk) so you can hear and feel each one without scanning
  a parcel to provoke it

All eleven scattered vibrate calls now route through one `wildVibrate()` gate,
which is the only place that decides whether to buzz and how hard. Settings
write straight to localStorage — a change takes effect on the very next scan.

**Honest limit:** the Web Vibration API has no amplitude, only duration. So
"strength" lengthens the pulse rather than driving the motor harder. Durations
are scaled and clamped to 15–400 ms — below 15 the motor doesn't spin up,
above 400 it reads as a fault. The screen says this rather than pretending
otherwise.

## 4. Back button menu (web)

New Options row listing what holding the hardware BACK key brings up, read
straight out of the APK rather than from memory:

- 3s — "Keep holding BACK…"
- 7s — "Keep holding — nearly there…"
- 10s — menu opens, titled *Kiosk mode is ON* / *Kiosk mode is OFF*

Five items: Leave kiosk mode (or Turn kiosk mode on), Exit app, Launch
StageNow, Android Settings, Change launcher (Home app), plus Close.

That menu is native, so the screen can only *list* it — it can't open it or
read the current kiosk state. Opening it from Settings would need a new JS
bridge and another APK.

---

## Verification

- smali round-trip of the untouched dex is **byte-identical** to the original
  (38,756 bytes) — toolchain proven before patching
- patched dex header SHA-1 and Adler-32 both check out
- disassembled the rebuilt dex and confirmed the call order is
  `enable() → silenceEngineFeedback() → read()` in both places
- every APK entry other than `classes.dex` and `assets/app.html` is
  byte-identical to the installed build
- signature verifies **v1, v2 and v3**; manifest intact (package `uk.wild.app`,
  HOME category, EMDK `uses-library`, `configChanges 0x40003FFF` still carrying
  mcc/mnc)
- all six inline scripts parse; Sound screen exercised in jsdom — toggle,
  persistence, strength scaling, clamping, and all five outcome tones

## Build stamps

`THIS_BUILD_NUM 178` · `BUILD_NAME v4.63` · `REPO_DATA_BUILD 131` ·
`REPO_DATA_HASH a5195d2e` · `REPO_DATA_ROWS 24102`

## Still outstanding

- Dataset audit: postcodes **W2 1RH** and **W2 3SS** (stray-row spans) need your eye
- The GitHub PAT from an earlier chat is still compromised — regenerate it
  narrowed to Issues: Read/Write
