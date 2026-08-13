# Wild App — v1.89 (build 85)

**This one is an APK.** Install it, then upload it to the repo.

## Install

1. On the PDA: **hold BACK for 10 seconds → "Leave"**. The kiosk fights the
   installer otherwise.
2. Install `WildApp.apk`. Same signing key as always, so it goes over the top —
   your walk corrections, added addresses and scan history are all kept.
3. Upload the same `WildApp.apk` to `ashkayuk-cmd/wildapp-updates` (main),
   replacing the old one, so the app can offer it to itself later.

Nothing else needs uploading. The addresses are unchanged (24,149, hash
`e71e95e7`) and they're baked into this APK.

`version.txt` and `content.json` in the repo are dead files — leave them alone.

---

## Why this had to be an APK

Two of the four things you asked for can't be done over the air.

**The way back from a revert** has to exist in the copy of the app *baked
inside the APK* — because reverting is precisely the act of booting that copy.
A downloaded update can't put a button there.

**The voice list** needed a change to the wrapper itself. The PDA's WebView has
no speech engine at all, so the app installs a fake one that hands speech to
Android. Its voice list was hard-coded empty — which is why ⚙️ Settings → Voice
was blank on the PDA while the same screen works in a browser.

---

## 1. PIN 1984 on corrections

Tapping **Wrong walk?** now asks for the PIN before the correction screens
open. Same keypad as the upload PIN, same four digits.

It then **stays unlocked for ten minutes**, so fixing three addresses in a row
doesn't mean typing it three times. It re-locks when the app restarts — the
window is deliberately held in memory, not saved.

Cancel puts the scan you were looking at back on screen, so a mistyped PIN
never strands you.

**What is *not* behind the PIN:** viewing your saved corrections (App version
screen → Walk corrections, and the Manage screen), Recent scans, and tapping a
red "also possible" card to pick it. None of those change your data.

## 2. PIN on anything that changes which version runs

Applying an update, dropping back to the built-in version, and going forward
again all ask for the PIN. **Checking doesn't** — looking costs nothing, and
making you type a PIN to find out whether an update exists would be theatre.

## 3. Reverting is no longer a one-way door

**Use built-in version** used to *delete* the downloaded copy. The only route
back was downloading it again, which needs signal — and the screen you landed
on was the old baked app, which had no button to do it with.

Now the copy is **set aside**, and a **Use latest update (build N)** button next
to it puts it back. No signal, no download, instant.

- The way-back button only appears when the set-aside copy actually **beats**
  what's running. A held copy older than the built-in app is clutter, not an
  option — the screen still says it's there, greyed into the text, but doesn't
  offer it.
- One generation is kept. Reverting twice overwrites the first, which is what
  you want: the thing you'd go back to is always the newest copy that ran.
- The addresses go with it — a downloaded address set is held and restored
  alongside the code, so a revert can't strand you on half of each.
- If storage is ever too full to hold the copy, the revert still happens the
  old way rather than failing. It just won't offer the way back.

Right after this install there's nothing to go back **to**: the build-84 copy
your PDA downloaded is now older than what's baked in, so the app ignores it.
The feature matters from the next over-the-air update onwards.

## 4. Voices in ⚙️ Settings

Settings → **🗣️ Voice** now lists the voices your phone actually has, the way
the browser version does: tap one to hear it, with **speed** and **pitch**
sliders under it. Your choice sticks.

Under the bonnet, two new methods on the speech bridge: one reports the
engine's installed voices, one selects the chosen one just before speaking.
Both fail soft — if the engine is still starting up, or isn't there at all, you
get the empty list and the default voice, exactly as before, and nothing
throws.

If the list is empty, the device genuinely has no voices installed: **Refresh**
re-asks the engine (it drops the cached list, not just the screen), and
Settings › Language & input › Text-to-speech is where Android adds them.

## 5. The ⟳ Update button is gone

Removed from the bottom-left row, as asked. The row now holds ⚙️ Settings and
the version pill.

Updating lives on the **App version** screen — tap the pill. That screen now
carries the Apply button when there's something waiting, next to Check now.

The automatic checks still run on their own (at boot, every three hours, and on
wake). When one finds something, **the version pill itself turns green and
reads "update ready"** — so a waiting update is still visible from the scanning
screen without a second button sitting in the corner.

---

## Checked before delivery

- **102 tests** on the exact `app.html` inside this APK: the button's absence
  and that the leftover wiring can't throw; the PIN on both flows, wrong PIN,
  the grace window, expiry, cancel; hold/restore including the storage-full
  fallback; every combination of buttons the version screen can show; the
  polyfill driven by a stub bridge (mapping, caching, refresh, voice selection,
  and a bridge that throws).
- **19 loader/update tests**: the loader baked into *this* APK boots the
  built-in app, ignores the now-old build-84 copy without binning it, boots a
  genuinely newer one, and still refuses a truncated copy, a copy that failed to
  start, and a copy of itself. Plus: this build fetches the repo as it stands
  today and correctly claims **nothing** — no phantom update, no needless 2 MB
  address download.
- **272-label render comparison** against build 84 — **0 differences**. Talbot
  Square, W2 1PN, the concierge rows, Lancaster Hall, the split-word and fused-
  postcode repairs, tracking codes, the not-in-W2 screen: all identical.
- **Dex diff**: exactly two added methods (`voices`, `setVoice`) and the
  replaced polyfill string. Every other method byte-identical, including the
  whole scanner and the sound values you asked to be restored in v1.78.
- Signed with your key — `2C:3A:BB:7A:B7:00:AF:19…`, verified v1/v2/v3.

## Stamps

`versionCode 85` · `THIS_BUILD_NUM=85` · `BUILD_NAME="v1.89"` ·
`BAKED_DATA_BUILD=85` · `REPO_APK_BUILD=85` · `REPO_DATA_BUILD=72` ·
`REPO_DATA_HASH="e71e95e7"` · `REPO_DATA_ROWS=24149` · loader `BAKED=85`

`WildApp.apk` — 1,115,057 bytes, sha256 `a70a07b206f66fd1…`
