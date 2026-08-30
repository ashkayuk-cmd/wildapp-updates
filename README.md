# Build 189 / v4.74

## 1. Brightness defaults — done
Now defaults to **auto brightness ON, slider at 50%** (was 60%). The error
fallback in `get()` also moved 60 → 50 so it can't disagree with itself.

Reminder on how this behaves: with auto ON the 50% isn't driving anything —
the device controls the screen. The 50% is the value that takes effect the
moment you switch auto OFF. That's what you asked for; just don't expect the
screen to visibly sit at 50% while auto is on.

Note this only affects **fresh installs / cleared data**. If this device has
already saved a brightness preference, that saved value wins and the new
default won't show. Clear app data if you want to see the new default.

## 2. Touch panel mode (gloves + screen protector) — partial
**Cannot be set by the app.** It's a Zebra device-level touch driver setting,
not an app setting. There's no API for it in the barcode EMDK this app links
against, and writing it directly would need `WRITE_SECURE_SETTINGS`, which a
sideloaded app can't grant itself. Anything I built for it would be guessing
at an undocumented key and failing silently.

**What I added instead:** a "Display & touch" row in Settings that opens
Android's own Display settings directly, so touch panel mode is two taps away
rather than buried. Set it there once — on TC56 it's usually
Display → Touch Panel Mode → "Glove and Finger" / "Screen Protector".

**To have it set automatically on every install**, that needs a Zebra
**StageNow** profile applied as part of device provisioning. That's the
supported route and it's outside the APK.

## 3. Default launcher prompt — cannot be suppressed from the app
Android deliberately requires the user to choose a HOME app; an app cannot
make itself the default launcher programmatically. This is core Android
security, not a Zebra or Wild App limitation.

Two ways to stop it recurring:
- When the chooser appears, pick **Wild App → Always** (not "Just once").
  That holds until the app is reinstalled — which is why it returns after
  every reset/reinstall.
- **Permanently:** set Wild App as **device owner** via StageNow, which locks
  the launcher with no prompt at all. Again, a provisioning step, not
  something the APK can do to itself.

## Stamps — all six checked
| Stamp             | Value    | Moved? |
|-------------------|----------|--------|
| `THIS_BUILD_NUM`  | 189      | yes |
| `BUILD_NAME`      | v4.74    | yes |
| `REPO_APK_BUILD`  | 189      | yes |
| `REPO_DATA_BUILD` | 131      | no — data untouched |
| `REPO_DATA_HASH`  | a5195d2e | no — data untouched |
| `REPO_DATA_ROWS`  | 24102    | no — data untouched |

Loader `BAKED` synced to 189.

## Deploy
1. Upload `index.html` to the repo.
2. Upload `WildApp.apk` to the repo.
3. Install on the device, or use "Update app from GitHub" — it should offer
   build 189.
