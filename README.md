# Wild App — v4.48 (APK build 163)

Fixes the two things you hit on v4.47: the black bar, and the notification
shade still pulling down.

Install over the top — same key, corrections and history kept. If you're
pushing it with your StageNow FileMgr + AppMgr profile, just point it at the
new file; nothing about the profile needs to change.

Re-pick Wild App as the launcher afterwards, as always.

---

## What was wrong and what changed

### The black bar — my mistake

v4.47 told the window to hide the status bar but never told the layout to draw
into the space it left behind, so the page still started below where the bar
used to be and you got a black strip. Fixed by adding the missing
`LAYOUT_FULLSCREEN` flag, so the app now fills the whole screen properly.

### The notification pull-down

v4.47 only used the system-UI visibility flags. Those hide the bar, but sticky
immersive deliberately still lets a swipe bring the shade back — I noted that
limit in the last README, and it turned out to be far too permissive in
practice.

v4.48 adds `FLAG_FULLSCREEN` on the window itself. That's a genuine fullscreen
window rather than just hidden bars, and on Zebra's Android 8 builds it stops
the shade being pulled down at all.

Both changes are in the same `applyImmersive()` method, which still runs on
`onCreate` and on every focus gain, and is still wrapped so it can't take the
app down.

**Behaviour outside the app is unchanged.** The notification bar works normally
the moment you leave Wild App or stop it — keep the notification setting
enabled in your StageNow UI Manager profile.

If a swipe still gets the shade open on your device, tell me — the next lever
is the kiosk's existing focus-loss bounce, which already catches the shade when
it does open, and I can make that more aggressive.

---

## Under the bonnet

Native-only build. The web layer is byte-identical to v4.47 apart from the four
build stamps.

- Smali diff vs build 162: **one method changed** (`applyImmersive`), nothing
  added, nothing removed. Two added instructions and one changed constant.
- APK diff vs build 162: only `classes.dex`, `AndroidManifest.xml`,
  `assets/index.html` (loader BAKED 162→163) and `assets/app.html` (stamps
  only) differ. The other 7 entries are byte-identical.
- Content diff: 8 lines, all of them build stamps.
- t47.cjs: 28/28 against the baked file.
- versionCode 163, platformBuildVersionCode still 24.
- Signed v1/v2/v3, cert 2C:3A:BB:7A:B7:00:AF:19… (CN "Wild App").
- Data unchanged: build 131, hash a5195d2e, 24,102 rows.

No render sweep this time — the resolver and the whole web layer are provably
untouched, so there is nothing for a label diff to find.
