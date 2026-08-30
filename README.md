# Brightness fix — build 188 / v4.73

## Why it did nothing
JavaScript bridge methods (`@JavascriptInterface`) run on a **background
WebView thread**, not the UI thread. `Window.setAttributes()` — the only way
to change screen brightness for an app window — has to be called on the UI
thread; called from anywhere else it silently does nothing.

My first version called it straight from the bridge method, so every set was
quietly discarded. The `try/catch` around it made it worse by hiding any
complaint. The toggle and slider were updating the saved preference correctly
the whole time — the screen just never got told.

**Fixed** by posting the change through `runOnUiThread()` so it lands on the
right thread. Saved values are unchanged, so whatever you'd already set will
take effect as soon as this build runs.

## Stamps for this release — all six checked
| Stamp             | Value    | Moved? |
|-------------------|----------|--------|
| `THIS_BUILD_NUM`  | 188      | yes |
| `BUILD_NAME`      | v4.73    | yes |
| `REPO_APK_BUILD`  | 188      | yes |
| `REPO_DATA_BUILD` | 131      | no — data untouched |
| `REPO_DATA_HASH`  | a5195d2e | no — data untouched |
| `REPO_DATA_ROWS`  | 24102    | no — data untouched |

Loader `BAKED` in the APK's own `index.html` also synced to 188.

## Deploy
1. Upload `index.html` to the repo.
2. Upload `WildApp.apk` to the repo (replacing the current one).
3. Install `WildApp.apk` on the device — or, since `REPO_APK_BUILD` is correct
   now, "Update app from GitHub" should offer build 188 and open the installer
   for you.

## Testing it
Settings → Brightness → turn **Auto brightness** off → drag the slider. The
screen should change as you drag, and hold that level after leaving the screen
and on the next restart. Turning Auto back on returns control to the device.
