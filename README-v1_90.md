# Wild App — v1.90 (build 86)

Two files, and a specific order. This carries everything from the last round
plus the Settings change you just asked for (Walk corrections in ⚙️ Settings),
and it fixes why the update wasn't showing up.

## Why the last one said "up to date"

The app never looks at the APK to decide whether there's an update — it reads
`index.html` and trusts what that file says is available. The APK you uploaded
was fine, but the `index.html` beside it was still the old one, advertising the
old build. So the app looked, saw nothing new announced, and said "up to date."

The `index.html` here fixes that: it announces build 86 **and** is itself the
new app, so applying it updates the code over the air straight away.

## Upload (both files, replacing the old ones)

| File | Upload as |
|---|---|
| `index.html` | `index.html` |
| `WildApp.apk` | `WildApp.apk` |

To `ashkayuk-cmd/wildapp-updates` (main). Addresses are unchanged (24,149).

## On the PDA

1. Tap the **version pill** (bottom-left) → **Check now**. This time it finds
   it. (The app you're running still has the old ⟳ Update button — using that
   works too.)
2. **Apply** → PIN **1984**. It downloads the new code and restarts. Everything
   below is now live.
3. It then flags the **APK** (the pill turns amber; the App version screen
   offers it). Install it when ready: **hold BACK 10 seconds → "Leave" → install
   over the top.** Same key, so corrections and history are kept. This last step
   only adds the "go back to the latest update" button — the rest already works
   from step 2.

If Check still says "up to date" right after uploading, give GitHub a minute to
catch up and tap again.

---

## Startup was doing 20 seconds of work nobody could see  *(the sluggishness)*

`finishBoot()` built the website's **Walks** browser at every single launch —
all 36 walks, every street, every building folder, all 24,149 addresses — as
about **102,000 DOM nodes, a 6.5 MB tree** — then walked the whole thing again
to decorate it.

On this build that screen cannot be opened. The `liteMode` stylesheet hides the
tab bar and the app is pinned to the handheld panel, so `#tab-routes` is
unreachable. Every launch paid for it anyway, and the tree then sat in memory
for the rest of the session with the page-wide change observer stepping over it.

Measured on the same file, same data: `finishBoot` went from **21,552 ms to
117 ms**. Everything else in boot put together was under 60 ms — this was
essentially all of it. (That's in a test harness, which is slower at DOM work
than the real WebView, so don't expect to save twenty literal seconds on the
PDA — but it was the overwhelming majority of startup, and it's now gone.)

The tree is now built **on demand**. Nothing on the handheld touches it. If a
build ever shows the tab bar again, it builds during boot exactly as before, and
every route into it (the Walks tab, `jumpToStreet`, rebuild-after-move) builds
it first. Verified: the handheld walk-to-streets screen, type-in lookup and
scanning all work with the tree never built.

## What's new since build 84

### Walk corrections is now in ⚙️ Settings

Settings now has a third row — **✏️ Walk corrections** — opening the same list
you could already reach from the version screen: every walk you've fixed and
every address you've added, each with Delete, plus the back-up and GitHub-upload
buttons. Its Back returns to Settings. The route from the App version screen
still works exactly as before.

### PIN 1984 on corrections

Tapping **Wrong walk?** asks for the PIN first, then stays unlocked for ten
minutes so a run of fixes isn't four PINs, and re-locks on restart. Viewing your
saved corrections isn't gated — only changing them.

### PIN on version changes

Applying an update, dropping back to the built-in version, and going forward
again all ask. Checking doesn't.

### Reverting is no longer one-way

**Use built-in version** used to delete the downloaded copy; now it's set aside,
and **Use latest update (build N)** puts it back instantly, no signal needed.
The way-back button only shows when the set-aside copy actually beats what's
running. (Right after this install there's nothing to go back to yet — it
matters from the next over-the-air update on.)

### Voices in ⚙️ Settings

Settings → **🗣️ Voice** lists the voices the phone actually has, with speed and
pitch, the same as the browser version. Empty list = the device has no voices
installed; Refresh re-asks, and Android adds them under Settings › Language &
input › Text-to-speech.

### The ⟳ Update button is gone

Removed from the bottom row. Updating lives on the App version screen (tap the
pill); when an update is waiting the pill turns green and reads "update ready".

---

## Checked before delivery

- **103 tests** on the exact `app.html` inside this APK — all green.
- **19 loader / update-check tests**: the loader boots the built-in app, ignores
  an older cached copy without binning it, boots a genuinely newer one, refuses
  a truncated / failed / self copy; and the running app applies this OTA file,
  caches build 86, and then correctly sees the build-86 APK.
- **272-label render comparison** vs build 84 — **0 differences**. Nothing about
  how labels resolve has moved.
- **Dex**: exactly the two added voice methods; `speak`, `stop`, the scanner,
  the restored v1.78 sound values — all byte-identical.
- Signed with your key, `2C:3A:BB:7A:B7:00:AF:19…`, verified v1/v2/v3.

## Stamps

`versionCode 86` · `THIS_BUILD_NUM=86` · `BUILD_NAME="v1.90"` ·
`REPO_APK_BUILD=86` · `REPO_DATA_BUILD=72` · `REPO_DATA_HASH="e71e95e7"` ·
`REPO_DATA_ROWS=24149`
APK: `BAKED_DATA_BUILD=86`, loader `BAKED=86`. OTA `index.html`:
`BAKED_DATA_BUILD=57` (so it runs on any older wrapper).

APK — 1,115,057 bytes, sha256 `fbf83b20881408cb…`
`index.html` — 595,788 bytes, sha256 `068bfd919bd5b72a…`

`version.txt` and `content.json` in the repo are dead files — leave them alone.
