# Wild App — data build 201 (v4.86), OTA only

## What changed
One address reassigned, from the master `wild_data.xlsx`:

| Address | Was | Now |
|---|---|---|
| The Spa Porchester Centre, Porchester Road, W2 5DP | 22 PORCHESTER | 24 QUEENSWAY |

Nothing else. 24,102 rows, identical order, no adds/removals, no street changes,
same 4 `_folder` entries, same 36 walks. Byte diff is one 13-char span.

## Files to upload (both, together)
- `wild_data.json`
- `index.html`

## Stamps
| Stamp | Live (was) | This build |
|---|---|---|
| THIS_BUILD_NUM | 200 | **201** |
| BUILD_NAME | v4.85 | **v4.86** |
| REPO_DATA_BUILD | 200 | **201** |
| REPO_DATA_HASH | 75fb7859 | **9ef79ff6** |
| REPO_DATA_ROWS | 24102 | 24102 (unchanged) |
| REPO_APK_BUILD | 199 | 199 (unchanged — see below) |
| BAKED_DATA_BUILD | 57 | 57 (unchanged) |

`index.html` differs from the live file on exactly 4 lines (605, 615, 631, 644) —
the four stamps above. The update sanity gate regexes in `readRepoStamps` are
byte-identical and were re-simulated: they read code 201 / data 201 / apk 199 /
rows 24102 / hash 9ef79ff6.

## APK build 199 is what is actually published
The repo `WildApp.apk` is versionCode **199**, `BUILD_NAME "v4.84"`,
`BAKED_DATA_BUILD 199`. The build-200 APK from the previous session was never
uploaded, and the live `index.html` still carries `REPO_APK_BUILD=199`.
So `REPO_APK_BUILD` has been left at 199 — raising it would make every device
report an APK update that does not exist in the repo. If you upload the
build-200 APK, restamp `REPO_APK_BUILD` to 200 at the same time.

This release is OTA-only: no native/dex change, no sideload needed.

## Verification performed
- Patch applied by literal string replacement on raw file text (not
  parse/re-serialise), so the fingerprint stays byte-exact.
- Hash function ported from the app's own `wildHash` (FNV-1a 32-bit over
  `charCodeAt(i)&0xff`, shift-add multiply) and validated against the live
  stamp: live file hashes to 75fb7859 as published. New file: 9ef79ff6.
  (Note: a plain FNV-1a over UTF-8 bytes gives d5744e66 — wrong, the file has
  non-ASCII characters. Use the charCode variant.)
- jsdom harness, all 6 app scripts loaded with zero script errors.
- 402-label resolve sweep (`findBestAddressMatch` across the whole dataset at
  even stride), old build+old data vs new build+new data: **0 unintended
  differences**, 0 nulls either side.
- Targeted spot check confirms the intended change only:
  - `"The Spa Porchester Centre, Porchester Road, W2 5DP"` → 22 PORCHESTER → **24 QUEENSWAY**
  - `"THE SPA PORCHESTER CENTRE W2 5DP"` → 22 PORCHESTER → **24 QUEENSWAY**
  - `"Porchester Centre W2 5DP"` → 22 PORCHESTER (unchanged)
  - `"W2 5DP"` bare → 22 PORCHESTER (unchanged best match)

## One behaviour change to be aware of
W2 5DP now spans two walks. `walksForPostcode("W25DP")` went from
`22 PORCHESTER, FIRM` to `22 PORCHESTER, 24 QUEENSWAY, FIRM`, so a scan that
yields only that postcode with no building text will now offer the walk picker
instead of resolving straight through. Labels that carry the Spa's name still
resolve confidently on their own.

## After uploading
- Allow 1–2 min for GitHub CDN edge cache before the raw URLs serve the new files.
- The first update check runs on the old code — check twice.

## Still outstanding (unchanged by this build)
- A-Z sheet lists BUCKHILL LODGE under "Bayswater Road" on walk 15; address data
  has it on "Hyde Park". Not touched here.
- GitHub PAT for the Issues upload feature still needs regenerating, scoped to
  Issues: Read and write only.
- Master `wild_data.xlsx` is now in sync with the shipped JSON — the Spa row was
  the only drift.
