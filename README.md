# Fix: "Update app from GitHub" not detecting the new APK

## What went wrong — my mistake
There are **six** release stamps, and I updated five. I missed
`REPO_APK_BUILD`, which is the one "Update app from GitHub" actually uses.

That button doesn't inspect the `.apk` file in the repo at all — it reads
`REPO_APK_BUILD` out of the repo's `index.html` and compares it against the
build baked into your installed APK. I'd bumped `THIS_BUILD_NUM` to 187 but
left `REPO_APK_BUILD` at 186, so the app compared 186 against your installed
187, concluded the repo was *behind*, and correctly reported nothing to
install.

(The comment above that constant documents this exact failure happening once
before, at v4.69. I walked into it anyway.)

## Fixed
`REPO_APK_BUILD` is now 187, matching. All six stamps for this release:

| Stamp             | Value      | Moved? |
|-------------------|------------|--------|
| `THIS_BUILD_NUM`  | 187        | yes    |
| `BUILD_NAME`      | v4.72      | yes    |
| `REPO_APK_BUILD`  | 187        | yes (was missed) |
| `REPO_DATA_BUILD` | 131        | no — data untouched |
| `REPO_DATA_HASH`  | a5195d2e   | no — data untouched |
| `REPO_DATA_ROWS`  | 24102      | no — data untouched |

## Deploy — order matters this time
1. **Upload `index.html`** to the repo first. This is the file that carries
   the corrected stamp, so it's the one that makes the button work.
2. **Upload `WildApp.apk`** to the repo, replacing the current one, so what
   the button downloads matches what the stamp advertises.
3. On the device, "Update app from GitHub" should now find build 187 and open
   the installer.

If you already manually installed the previous 187 APK, the button will say
"Already on build 187 — tap again to install anyway", which is correct and
harmless; you don't need to reinstall. The point of this fix is that *future*
APK releases will be detected properly.

## Note on the two buttons
- **Check now** → checks `index.html` / `wild_data.json` (the web layer),
  applies automatically on next launch.
- **Update app from GitHub** → checks `REPO_APK_BUILD`, and if newer,
  downloads the repo's `WildApp.apk` and opens Android's installer. Still
  needs your tap to confirm the install — Android won't silently install —
  but it does mean you don't have to transfer the file by hand.
