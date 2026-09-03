# Wild App — build 204 (v4.89), OTA only

Four changes, all web-layer. No native/dex change, no sideload.

## 1. Labels (new)
| Where | Was | Now |
|---|---|---|
| Main screen button | ⌨ Type a postcode | **⌨ Type a postcode to show walk** |
| Postcode screen heading | Type a postcode | **Type a postcode to show walk** |
| Main screen button | ⌨ Type a Street or Building name | **⌨ Search Street or building** |
| Street screen heading | Search a street or building | **Search Street or building** |

The two main-screen buttons exist twice in the file — once as static HTML and
once in `handIdleActionsHtml()`, which rebuilds the idle screen on every Back
route. Both copies were changed, so the new labels survive going Back as well
as a fresh start. Confirmed by reading the rendered buttons and the rebuilt
HTML separately.

Left alone (different screens, not what you asked for — say if you want them
matched up):
- `Search a street or building…` placeholder in the **Walks simple** search box
- `Type a postcode` under "Find any address" on the scan tab's empty state
- `Scan a code — or type a postcode / address here…` textarea placeholder

## 2. Street/building keypad scrolls with the page (build 203)
`#handStTop` was `position:sticky; top:0` and stayed pinned while only the hit
list moved. It's a normal block now — scroll up and the keyboard goes with it.

The postcode keypad, the **House number** pad and the **Flat or door number**
pad are still pinned.

## 3. Postcode keypad searches on the 5th character (build 202)
`W2` is printed, three characters get typed, and the pad greys out any key that
can't reach a real postcode — so the fifth keystroke runs the lookup. **Look up**
stays as a fallback for a partial like `W2 5D`; backspacing within 90 ms cancels.

## 4. Data build 201
| Address | Was | Now |
|---|---|---|
| The Spa Porchester Centre, Porchester Road, W2 5DP | 22 PORCHESTER | 24 QUEENSWAY |

24,102 rows, identical order, no adds/removals, no street changes, same 4
`_folder` entries, same 36 walks. The xlsx had no other drift. W2 5DP now spans
two walks, so a bare-postcode lookup on it offers the walk picker rather than
resolving straight through; labels carrying the Spa's name still resolve
confidently.

**Nothing in items 2–4 has been published yet** — live is still build 200 / v4.85
with data build 200.

## Files to upload (both, together)
- `index.html`
- `wild_data.json`

## Stamps
| Stamp | Live (was) | This build |
|---|---|---|
| THIS_BUILD_NUM | 200 | **204** |
| BUILD_NAME | v4.85 | **v4.89** |
| REPO_DATA_BUILD | 200 | **201** |
| REPO_DATA_HASH | 75fb7859 | **9ef79ff6** |
| REPO_DATA_ROWS | 24102 | 24102 (unchanged) |
| REPO_APK_BUILD | 199 | 199 (unchanged — see below) |
| BAKED_DATA_BUILD | 57 | 57 (unchanged) |

Update gate re-simulated on the delivered file: code 204 / data 201 / apk 199 /
rows 24102 / hash 9ef79ff6. `readRepoStamps` regexes byte-identical.

## APK build 199 is what is actually published
Repo `WildApp.apk` is versionCode **199**, `BUILD_NAME "v4.84"`,
`BAKED_DATA_BUILD 199`. The build-200 APK was never uploaded and live
`index.html` still carries `REPO_APK_BUILD=199`, so it stays at 199 — raising it
would advertise an APK that isn't in the repo. Upload the 200 APK and I'll
restamp to match.

## Verification performed
- Diff from the previous build is 5 lines: two stamps, the static button pair,
  the two headings and the rebuilt button pair. Nothing else changed.
- jsdom harness, all 6 app scripts loaded, zero script errors.
- Rendered labels read back exactly: `⌨ Type a postcode to show walk`,
  `⌨ Search Street or building`, headings `Type a postcode to show walk` and
  `Search Street or building`. Both buttons still carry their onclick, the
  postcode pad still draws 36 keys and the street pad 26. Zero occurrences of
  the old `Type a Street or Building name` string remain.
- Postcode keypad suite still green: single fire on `5`,`D`,`P`; backspace
  cancels; suggestion chip fires once; two characters don't fire but **Look up**
  does; re-arms on a second postcode; all keys dead after the third character.
- Street screen suite still green: `#handStTop` has no `position` property,
  typing `WESTBOURNE` gives the same 14 hits and a byte-identical 11,001-char
  hit list, ⌫ and clear behave, and the other three pads still report `sticky`.
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
