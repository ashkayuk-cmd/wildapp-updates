# Wild App — build 203 (v4.88), OTA only

Three changes, all web-layer. No native/dex change, no sideload.

## 1. Street/building keypad scrolls with the page (new)
On **Search a street or building**, the display and keypad block (`#handStTop`)
was `position:sticky; top:0`, so it stayed pinned to the top of the screen and
only the hit list moved underneath. It's now a normal block: scroll up and the
keyboard scrolls up with everything else, giving the whole screen to the
results. Scroll back down and it's there again.

Nothing else about the screen changed — 26 letter keys, space, ⌫ and clear all
behave as before, and the hit list is byte-identical for the same typed text.

Left alone deliberately (not what you asked for, but the same pinning applies —
say the word and they go too):
- the postcode keypad (`#handTypeTop`)
- the **House number** pad on a street's address list (`#handStNumTop`)
- the **Flat or door number** pad on a named building (`#handStBTop`)

## 2. Postcode keypad searches on the 5th character (build 202, not yet published)
`W2` is printed, three characters get typed, and the pad greys out any key that
can't reach a real postcode — so the fifth keystroke runs the lookup. **Look up**
is no longer a tap you have to make; it stays as a fallback for a partial like
`W2 5D`, and backspacing within 90 ms cancels the pending search.

## 3. Data build 201 (not yet published)
| Address | Was | Now |
|---|---|---|
| The Spa Porchester Centre, Porchester Road, W2 5DP | 22 PORCHESTER | 24 QUEENSWAY |

24,102 rows, identical order, no adds/removals, no street changes, same 4
`_folder` entries, same 36 walks. The xlsx had no other drift.

W2 5DP now spans two walks, so a bare-postcode lookup on it offers the walk
picker rather than resolving straight through. Labels carrying the Spa's name
still resolve confidently on their own.

## Files to upload (both, together)
- `index.html`
- `wild_data.json`

## Stamps
| Stamp | Live (was) | This build |
|---|---|---|
| THIS_BUILD_NUM | 200 | **203** |
| BUILD_NAME | v4.85 | **v4.88** |
| REPO_DATA_BUILD | 200 | **201** |
| REPO_DATA_HASH | 75fb7859 | **9ef79ff6** |
| REPO_DATA_ROWS | 24102 | 24102 (unchanged) |
| REPO_APK_BUILD | 199 | 199 (unchanged — see below) |
| BAKED_DATA_BUILD | 57 | 57 (unchanged) |

Update gate re-simulated on the delivered file: code 203 / data 201 / apk 199 /
rows 24102 / hash 9ef79ff6. `readRepoStamps` regexes byte-identical.

## APK build 199 is what is actually published
Repo `WildApp.apk` is versionCode **199**, `BUILD_NAME "v4.84"`,
`BAKED_DATA_BUILD 199`. The build-200 APK was never uploaded and live
`index.html` still carries `REPO_APK_BUILD=199`, so it's left at 199 — raising
it would advertise an APK that isn't in the repo. Upload the 200 APK and I'll
restamp to match.

## Verification performed
- Diff from the previous build is 3 lines: two stamps and the `#handStTop`
  style. Nothing else in 803 KB changed.
- jsdom harness, all 6 app scripts loaded, zero script errors.
- Street screen, driven through the real DOM: `#handStTop` has no `position`
  property at all (previous build reported `sticky`), 26 keys present, typing
  `WESTBOURNE` gives the same buffer, same 14 hits and a byte-identical
  11,001-char hit list as before; ⌫ → `WESTBOURN`, clear → empty, pad intact.
- The other three pads still report `sticky`, confirming the change is scoped
  to the one screen.
- Postcode keypad suite re-run and still green: single fire on `5`,`D`,`P`;
  backspace cancels; suggestion chip fires once; two characters don't fire but
  **Look up** does; re-arms on a second postcode.
- 402-label resolve sweep vs live build+live data: **0 unintended differences**.
- Data patched by literal string replacement on raw file text; hash from the
  app's own `wildHash` (FNV-1a 32-bit over `charCodeAt(i)&0xff`), validated by
  reproducing the live `75fb7859` stamp first. Plain UTF-8 FNV-1a gives
  `d5744e66` and is wrong — the file has non-ASCII characters.

## After uploading
- Allow 1–2 min for GitHub CDN edge cache.
- First update check runs on the old code — check twice.

## Still outstanding
- A-Z sheet lists BUCKHILL LODGE under "Bayswater Road" on walk 15; address
  data has it on "Hyde Park".
- GitHub PAT for the Issues upload feature needs regenerating, scoped to
  Issues: Read and write only.
