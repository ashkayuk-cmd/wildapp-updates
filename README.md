# Wild App — v4.52 (content build 167)

**Upload `index.html` to the repo. No APK, no install.**

Upload as exactly `index.html` and press the green **Commit changes** button.
The change won't show on the first check after publishing — the old code runs
that check. Check again.

---

## What changed

The website-version bar at the bottom now shows **only on the main scanning
screen**. It disappears as soon as anything else is on screen — a scan result,
type a postcode, type a street, walks, recent scans, corrections, any sub-screen
— and comes back when you return to the main screen.

### Why it was on every page

The bar sits outside the scrolling middle of the screen, so it survived every
render rather than being replaced along with the content. Nothing was
refreshing it, so it just stayed put.

### How it's done

The rule hangs off `afterRender`, the hook that already runs after every
handheld render, so it covers scans, sub-screens, Back journeys and the
tap-title-home route without touching any of them individually. The bar shows
only when the idle template is on screen and no sub-screen has claimed the
result area.

The observer that drives `afterRender` also now watches the sub-screen marker
attribute, not just the content — otherwise a screen that sets the marker
without changing the content underneath wouldn't have triggered a refresh.

The bar now starts hidden in the markup and is shown once the state is judged,
so it can't flash on during boot.

---

## Checks run

- t52.cjs: 19/19, walking the real journeys — idle, scan, back home via the
  title, four sub-screens each followed by a return home, and the marker set
  and cleared directly. Build 166 fails exactly the visibility checks, showing
  the bar on every one of those screens: the bug reproducing.
- 250-label render diff vs build 166: **0 differences.**
- All six script blocks syntax-checked.
- Update sanity gate accepts this file and is byte-identical to build 166.
- 26 changed lines.
- Stamps: THIS_BUILD_NUM 167, REPO_APK_BUILD 164, BAKED_DATA_BUILD 57. Data
  untouched (build 131, a5195d2e, 24,102 rows) — no `wild_data.json` upload.

### Test note

Two failures in the first run were my test calling `wildHome()` when the app
exposes it as `window.__wildHome` — test bug, not app bug. Worth remembering:
the home routine is not a plain global.
