# Wild App — APK v1.95 (app build v4.69 / 184)

Supersedes every earlier build in this session. Install this one.
The native side moved in this build (v1.95), so the APK is required for the
new volume sliders and the scanner-beep switch.

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

## 1. "Repo has build 170, you have 182" — fixed

There is a **sixth** release stamp, `REPO_APK_BUILD`, and it had been sitting
at **170** through every release. "Update app from GitHub" compares the repo's
APK build against the installed one, so it correctly reported the repo as
behind — the stamp was lying, not the check.

The five I had been keeping in step (`THIS_BUILD_NUM`, `BUILD_NAME`,
`REPO_DATA_BUILD`, `REPO_DATA_HASH`, `REPO_DATA_ROWS`) all describe the code
and the data. `REPO_APK_BUILD` describes the APK next to them, and it is
genuinely a separate number: an OTA-only release publishes a new `index.html`
with no new APK, and this must then stay on the last APK's build. It only moves
when an APK is actually uploaded — which is exactly why it got missed.

Both are 184 in this release, because the APK is being published alongside.
Reproduced the message from your repo's current file (`apk:170` vs installed
182 → "Repo has build 170, you have 182"), then confirmed this release gives
"Opening installer… (build 184)" and, once installed, "Already on build 184".

**Release checklist is now six stamps, not five.**

## 2. Device volume + scanner beep in Sound & vibration

New `WildAudio` bridge. Options → Sound & vibration now carries the sliders the
notification shade shows — **Media, Notifications, Ring, Alarm, System** — because
in kiosk mode there's no shade to pull down and they were otherwise unreachable
without leaving the app.

These read and write the real `AudioManager` streams. Nothing is cached: values
are re-read every time the screen opens, so pressing the hardware volume keys
and coming back shows the truth rather than a stale copy.

The setter returns the level the stream **actually** ended up at, not the one
asked for. Android refuses Ring and Notifications while Do Not Disturb is on,
and this app deliberately doesn't ask for notification-policy access — so a
refused write shows as the slider springing back, which is honest, rather than
a slider that moves while nothing happens. Tested with a DND mock: Media 7→12
sticks, Ring 2→6 springs back to 2, over-max clamps.

**Scanner's own beep** is now a switch rather than a decision. v1.93 silenced
Zebra's decode beep outright to kill the double sound; it's a stored preference
(`wild_prefs/scanner_beep`) now, still defaulting to OFF. Flipping it re-pushes
to the live scanner via `applyBeepPref()`, so it lands on the next trigger pull
rather than the next re-acquire.

Both sections are omitted entirely on a wrapper without the bridge — a slider
that does nothing is worse than no slider.

One caveat: turning the beep back **on** restores EMDK's documented default
feedback URI rather than a remembered one, because by then the blank has
already been written over whatever was there. If it sounds like the
notification tone rather than the classic Zebra beep, that's why — tell me and
I'll capture the original URI at first acquire instead.

## 3. Options: "Restart scanner" removed

Gone at Ash's request. `window.wildScanner` itself is untouched — the
automatic fix when the app wakes from sleep still runs, which is where the
scanner most often loses its arming. The manual fallback is unchanged: RESET
APP in the title bar.

Options is now eleven rows, still A–Z. Checked every row still opens its own
screen after the removal — the list is sorted and then wired by index, so a
deletion is exactly the kind of change that can drift the handlers.

## 4. Exact-address beep sounded like it repeated — fixed (web)

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

## 5. Back button menu — the rows now RUN (new in this build)

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

## 6. The voice stutter — fixed (web, was v4.64)

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

## 7. The double scan sound — fixed (native, was v1.93)

The shipped APK **never configured the scanner at all** — no `ScannerConfig`
anywhere — so Zebra's decode beep ran at factory default and the app's outcome
tone played on top.

New `silenceEngineFeedback()` blanks `scannerParams.decodeAudioFeedbackUri` and
clears `decodeHapticFeedback`, called between `enable()` and `read()` in both
`acquireScanner()` and `resume()` — setConfig needs the scanner enabled and
must run before the read, and re-enabling after a pause can hand back the
defaults. Wrapped so a firmware missing either field loses the silencing, not
the scanner.

## 8. A double *buzz* found on the way (web)

Three paths — your correction, an override, the Junction Mews picker — buzzed
40 ms themselves and then called `outcomeTone()`, which buzzes again. Gone;
`outcomeTone()` owns the haptic, as its comments always claimed.

## 9. Sound & vibration (web)

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

`THIS_BUILD_NUM 184` · `BUILD_NAME v4.69` · `REPO_APK_BUILD 184` ·
`REPO_DATA_BUILD 131` · `REPO_DATA_HASH a5195d2e` · `REPO_DATA_ROWS 24102`

Six stamps. `REPO_APK_BUILD` only moves when an APK is uploaded; the other five
move on every release.

## Still outstanding

- Dataset audit: postcodes **W2 1RH** and **W2 3SS** (stray-row spans)
- The GitHub PAT from an earlier chat is still compromised — regenerate it
  narrowed to Issues: Read/Write
