# Wild App — v1.92 (build 87)

**One file, no APK, no install.**

| File | Upload as |
|---|---|
| `index.html` | `index.html` |

To `ashkayuk-cmd/wildapp-updates` (main), replacing the current one. Leave
`WildApp.apk` alone — build 86 is correct and this build still points at it.
Addresses unchanged (24,149).

On the PDA: version pill → **Check now** → **Apply** (PIN 1984). It restarts on
build 87.

---

## 1. The PIN asks every time

The ten-minute unlock is gone. Every time you go back into a correction it asks
again — no window, no remembering, and nothing carried over between one fix and
the next.

## 2. The corrections list is behind the PIN too

Tapping **⚙️ Settings → ✏️ Walk corrections** now asks for 1984 before the list
opens.

I've done the same to the **other two doors into that same screen**, because
guarding one and leaving two open wouldn't actually protect anything:

- **Manage my corrections** on the scan result;
- **Manage corrections (delete, back up, upload)** on the App version screen.

All three land on the screen that deletes corrections and uploads them to
GitHub, so all three ask.

Cancelling always puts you back where you came from — Settings, the scan, or
the version screen respectively.

### What still doesn't ask

**Viewing** your corrections read-only on the App version screen, where they're
listed newest-first with no delete buttons. That's a display, not a way to
change anything, and it's the quick "what have I fixed?" glance.

---

## Checked before delivery

- **106 feature tests**, including: the PIN asked on a second correction
  immediately after the first, all three routes into the corrections screen
  asking, each cancel path returning to the right screen, and a wrong PIN
  keeping you on the pad.
- **43 stability tests** and the loader/update checks, all green.
- **272-label render comparison** against build 84 — **0 differences**.
- The build you're running now (86) was pointed at this file: it takes it as a
  code update, caches build 87, and does **not** start claiming a phantom app
  update — `REPO_APK_BUILD` stays 86, which is the APK actually in the repo.

## Stamps

`THIS_BUILD_NUM=87` · `BUILD_NAME="v1.92"` · `REPO_APK_BUILD=86` ·
`REPO_DATA_BUILD=72` · `REPO_DATA_HASH="e71e95e7"` · `REPO_DATA_ROWS=24149` ·
`BAKED_DATA_BUILD=57`

`index.html` — 601,944 bytes, sha256 `33e86529927f112e…`
