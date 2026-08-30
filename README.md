# Wild App — APK v1.94 (app build v4.67 / 182)

Supersedes every earlier build in this session. Install this one.
The native side is unchanged since v1.94 — only the web layer moved.

## Upload to `ashkayuk-cmd/wildapp-updates`

| File | Needed? |
|---|---|
| `WildApp.apk` | **Yes** — install on the TC56 |
| `index.html` | **Yes** — OTA copy of the same web build |
| `wild_data.json` | No — dataset unchanged (24,102 rows, hash `a5195d2e`) |

Signed with your existing key, verified against the signature on the installed
APK — it goes straight over the top. Corrections, Today's Walk and settings all
survive.

---

## 1. Options: "Restart scanner" removed

Gone at Ash's request. `window.wildScanner` itself is untouched — the
automatic fix when the app wakes from sleep still runs, which is where the
scanner most often loses its arming. The manual fallback is unchanged: RESET
APP in the title bar.

Options is now eleven rows, still A–Z. Checked every row still opens its own
screen after the removal — the list is sorted and then wired by index, so a
deletion is exactly the kind of change that can drift the handlers.

## 2. Exact-address beep sounded like it repeated — fixed (web)

Only the EXACT outcome doubled, which is what pointed at the cause: a stray
engine beep would have doubled every outcome, not one of them.

EXACT was two separate notes — 1500 Hz, then a 50 ms silence, then 1900 Hz.
It was meant to read as one rising chirp, but two notes of similar length a
short interval apart with a gap in the middle is heard as a beep that repeats.

Now ONE note that glides 1500→1900 across 170 ms. Same rising shape, same
length, unmistakably a single beep — and still nothing like BUILDING, which is
flat at 1400. PICKER stays a genuine double on purpose: that one means "look at
the screen", and sounding repeated is the point.

Verified by capturing the oscillator schedule: exact = 1 note 1500→1900,
building = 1 flat note, picker = 2 notes, none = 1 note, and a FIRM walk still
adds its low tail.

## 3. Back button menu — the rows now RUN (new in this build)

New `WildKiosk` bridge in the APK. Tapping a row does exactly what picking it
off the native dialog does, because the bridge hands the index to the dialog's
**own listener** (`MainActivity$12`) rather than reimplementing the actions.

That matters for more than tidiness: the shipped listener also opens a
120-second `allowLeaveUntil` grace window, which is what stops the kiosk
yanking itself back to the front while you're in Settings or StageNow. A
reimplementation would have quietly lost it and the rows would have half-worked.

- **StageNow**, **Android Settings**, **kiosk toggle** — run on tap
- **Exit app** and **Change launcher** — ask first, in an in-page sheet.
  Not `window.confirm()`: a system dialog steals window focus, and the kiosk
  answers a focus loss by dismissing system windows and pulling itself back —
  the two would fight, exactly as the on-screen keypad used to.
- The screen now reads the **live kiosk state**, so row 0 says the right thing
  and the 10s line says which state you're actually in. Toggling redraws it.
- **Open the real menu instead** button, if you'd rather see the dialog.

On an older wrapper the rows stay a plain list with a line saying why. A tap
that silently does nothing is worse than a row that never looked tappable.

## 4. The voice stutter — fixed (web, was v4.64)

`speakText()` scheduled speaking on a 350 ms timer and never kept hold of it,
so two requests close together left **two timers armed**. The native TTS bridge
speaks with `QUEUE_FLUSH`, so the second landed mid-word and flushed the first
— a stutter, not two readings.

There was a guard meant to catch this, but it asked
`speechSynthesis.speaking` / `.pending`, and the native shim hardcodes both to
`false` — so it never fired once on the PDA.

Now single-flight: a pending timer is cancelled before a new one is armed, and
the same phrase inside 1.5s is dropped rather than restarted. A *different*
walk always speaks — that's a new answer and must be heard. Reproduced against
a mock of the shim (2 utterances, 1 flushed) and confirmed fixed (1, 0).

## 5. The double scan sound — fixed (native, was v1.93)

The shipped APK **never configured the scanner at all** — no `ScannerConfig`
anywhere — so Zebra's decode beep ran at factory default and the app's outcome
tone played on top.

New `silenceEngineFeedback()` blanks `scannerParams.decodeAudioFeedbackUri` and
clears `decodeHapticFeedback`, called between `enable()` and `read()` in both
`acquireScanner()` and `resume()` — setConfig needs the scanner enabled and
must run before the read, and re-enabling after a pause can hand back the
defaults. Wrapped so a firmware missing either field loses the silencing, not
the scanner.

## 6. A double *buzz* found on the way (web)

Three paths — your correction, an override, the Junction Mews picker — buzzed
40 ms themselves and then called `outcomeTone()`, which buzzes again. Gone;
`outcomeTone()` owns the haptic, as its comments always claimed.

## 7. Sound & vibration (web)

The Sound screen rebuilt in the shape of Android's own page: beep volume,
**Vibrate on scan** switch, **Light / Normal / Strong**, and a test row per
outcome so each tone can be heard without scanning a parcel to provoke it.
All eleven scattered vibrate calls now route through one `wildVibrate()` gate.

Honest limit, stated on the screen: the Web Vibration API has no amplitude,
only duration, so "strength" lengthens the pulse. Clamped 15–400 ms — below 15
the motor doesn't spin up, above 400 it reads as a fault.

---

## Verification

- smali round-trip of the untouched dex is **byte-identical** to the original
  (38,756 bytes) — toolchain proven before any patching
- rebuilt dex disassembled and checked: `enable() → silenceEngineFeedback() →
  read()` in both places; `KioskBridge` carries three `@JavascriptInterface`
  methods; `WildKiosk` registered alongside `WildTTS` and `WildReset`
- every APK entry other than `classes.dex` and `assets/app.html` is
  byte-identical to the installed build
- baked `app.html` is byte-identical to the OTA `index.html`
- signature verifies **v1, v2, v3**; manifest intact (`uk.wild.app`, HOME
  category, EMDK `uses-library`, `configChanges 0x40003FFF` still carrying
  mcc/mnc)
- jsdom: both wrapper cases (bridge / no bridge), both confirm dialogs
  including Cancel, the kiosk toggle redraw, and all five Sound test tones

## Build stamps

`THIS_BUILD_NUM 182` · `BUILD_NAME v4.67` · `REPO_DATA_BUILD 131` ·
`REPO_DATA_HASH a5195d2e` · `REPO_DATA_ROWS 24102`

## Still outstanding

- Dataset audit: postcodes **W2 1RH** and **W2 3SS** (stray-row spans)
- The GitHub PAT from an earlier chat is still compromised — regenerate it
  narrowed to Issues: Read/Write
