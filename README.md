# Wild App — build 206 (v4.91), OTA only

Five changes, all web-layer. No native/dex change, no sideload.

## 1. UPDATE IN PROGRESS screen (new)
Applying an update is a reload through the loader, and on a kiosk PDA that just
looks like the app dying — a blank screen for a second or two with nothing to
say why. There's now a full-screen red panel with **UPDATE** in big bold white
letters and **IN PROGRESS** underneath it.

- Red `#C40000`, white text, `UPDATE` at 56 px and `IN PROGRESS` at 34 px, both
  weight 900, centred, `z-index` at the maximum so nothing sits over it.
- Painted **before** the reload is requested (400 ms head start, measured at
  ~5 ms to paint), so it's on screen for the whole gap rather than flashing.
- The reload throws the page away, so a flag is left in storage and the build
  that comes back up puts the same screen straight back, then clears it once
  the app is actually on screen. That covers the blank loader gap end to end.
- The flag is cleared the instant it's read, and ignored if older than 30 s, so
  a stale one can never show the screen twice or strand a device.
- Safety net: if the reload somehow never happens, the screen removes itself
  after 12 s rather than leaving a red brick.
- Inline styles appended to `<body>` — doesn't depend on the stylesheet
  surviving the reload, and no `inset` or `clamp()` (this WebView is Android 8).

**The APK path is deliberately excluded.** A new APK still has to be downloaded
and installed by hand, and that isn't a reload — showing "update in progress"
over Android's installer would be wrong. Only the over-the-air app/address
apply shows it. Say if you want it on the APK download too.

## 2. Labels (builds 204, 206)
| Where | Now |
|---|---|
| Main button + postcode screen heading | **Type a postcode to show walk** |
| Main button + street screen heading | **Search Street, Building or Business Name** |

Both main-screen buttons exist twice (static HTML and `handIdleActionsHtml()`,
which rebuilds the idle screen on every Back) — both copies changed.

Untouched, different screens: the **Walks simple** search placeholder, the
"Find any address" empty state on the scan tab, the scan textarea placeholder.

## 3. Street/building keypad scrolls with the page (build 203)
`#handStTop` was `position:sticky; top:0`. Now a normal block — scroll up and
the keyboard goes with it. The postcode, House number and Flat/door pads are
still pinned.

## 4. Postcode keypad searches on the 5th character (build 202)
The fifth keystroke runs the lookup; **Look up** stays as a fallback for a
partial like `W2 5D`, and backspacing within 90 ms cancels.

## 5. Data build 201
| Address | Was | Now |
|---|---|---|
| The Spa Porchester Centre, Porchester Road, W2 5DP | 22 PORCHESTER | 24 QUEENSWAY |

24,102 rows, identical order, no adds/removals, no street changes, same 4
`_folder` entries, same 36 walks. W2 5DP now spans two walks, so a
bare-postcode lookup on it offers the walk picker; labels carrying the Spa's
name still resolve confidently.

**Items 2–5 are still unpublished** — live is build 200 / v4.85, data build 200.

## Files to upload (both, together)
- `index.html`
- `wild_data.json`

## Stamps
| Stamp | Live (was) | This build |
|---|---|---|
| THIS_BUILD_NUM | 200 | **206** |
| BUILD_NAME | v4.85 | **v4.91** |
| REPO_DATA_BUILD | 200 | **201** |
| REPO_DATA_HASH | 75fb7859 | **9ef79ff6** |
| REPO_DATA_ROWS | 24102 | 24102 (unchanged) |
| REPO_APK_BUILD | 199 | 199 (unchanged — see below) |
| BAKED_DATA_BUILD | 57 | 57 (unchanged) |

Update gate re-simulated: code 206 / data 201 / apk 199 / rows 24102 / hash
9ef79ff6. The app's own `liveHtmlLooksSane()` was extracted and run against the
delivered file — returns **true**, so devices will accept this as an update.

## APK build 199 is what is actually published
Repo `WildApp.apk` is versionCode **199**, `BUILD_NAME "v4.84"`,
`BAKED_DATA_BUILD 199`. The build-200 APK was never uploaded and live
`index.html` still carries `REPO_APK_BUILD=199`, so it stays at 199 — raising it
would advertise an APK that isn't in the repo.

## Verification performed
- Diff from the previous build is 5 lines: two stamps, the static street button,
  the street heading and the rebuilt street button.
- jsdom harness, all 6 app scripts loaded, zero script errors.
- Update screen, driven through the real functions:
  - `liveApplyNow()` → panel present, `position:fixed`, `background: rgb(196,0,0)`,
    `z-index: 2147483647`, parent `BODY`, two children reading `UPDATE` and
    `IN PROGRESS`, font sizes 56/34 px, weight 900/900, no `inset`, no `clamp()`
  - panel is up **before** any navigation is attempted (0 navigations at 200 ms,
    1 by 800 ms), and stays up through the attempt
  - fresh boot flag → panel present at boot, flag cleared on read, gone after the
    hide timers
  - stale flag (2 min old) → panel never shows, flag still cleared
  - `applyUpdateNow("apk")` → no panel, APK download still starts
  - `applyUpdateNow("code")` → panel shows
  - 12 s safety hide fires
- Postcode keypad suite still green (single fire, backspace cancels, chip fires
  once, partial + **Look up** works, re-arms, keys dead after third character).
- Street screen suite still green (`#handStTop` unpinned, same 14 hits and
  byte-identical 11,001-char hit list, other three pads still `sticky`).
- Label suite still green: buttons read `⌨ Type a postcode to show walk` and
  `⌨ Search Street, Building or Business Name`, headings match, both onclicks
  intact, 36 + 26 keys. The rebuilt idle HTML from `handIdleActionsHtml()`
  carries both new labels and zero occurrences of any old string.
- 402-label resolve sweep vs live build+live data: **0 unintended differences**.
- Data patched by literal string replacement on raw file text; hash from the
  app's own `wildHash` (FNV-1a over `charCodeAt(i)&0xff`), validated against the
  live `75fb7859` stamp first.

## After uploading
- Allow 1–2 min for GitHub CDN edge cache.
- First update check runs on the old code — check twice. The red screen will
  only appear from the *next* update onwards, since the build doing the applying
  has to be one that knows about it.

## Still outstanding
- A-Z sheet lists BUCKHILL LODGE under "Bayswater Road" on walk 15; address
  data has it on "Hyde Park".
- GitHub PAT for the Issues upload feature needs regenerating, scoped to
  Issues: Read and write only.
