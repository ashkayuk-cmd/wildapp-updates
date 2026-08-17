# Wild App v4.18 — build 132 — Today's walk, second pass

**Upload `index.html` on its own.** No data file this time — the data is
unchanged (build 131, hash `a5195d2e`), and the APK is still 115.

If you haven't yet uploaded the build 131 files, upload `wild_data.json` from
the last hand-over alongside this. This `index.html` replaces that one and
carries everything in it.

---

## The three things you asked for

**1. The count resets.**
It used to count every scan since midnight, whatever you did with the setting.
Now it counts from the moment you picked, and the line reads *"N scanned since
you picked"*. Clear your walk and the count goes with it — pick again and you
start from zero.

Adding a *second* walk part-way through the round keeps the running count, so
you don't lose the morning's numbers. Taking one off starts it again, since the
figures on screen would otherwise be counting a walk you're no longer doing.

**2. More than one walk.**
Tap as many as you're carrying. Tap one again to take it off. A scan for any of
them is fine; anything else still gets the red warning and the spoken *"not
your walk"*. The red band lists all the walks you're on.

**3. Blue when there's more than one.**
One walk picked → green, as before. Two or more → the ticks, borders and the
header turn blue, so a glance tells you which mode you're in. The confirmation
strip on a good scan reads **✓ ONE OF YOUR WALKS** instead of **✓ YOUR WALK**.
The red warning is unchanged either way.

Your existing setting from build 131 is read straight through — it just becomes
a list of one, so nothing is lost when you update.

---

## How it was checked

- **t54.cjs — 35/35**, ten of them new: multi-select, the colour switch, the
  tally counting from the pick, resetting on clear and on removal, keeping the
  count when a walk is added, and the old single-walk setting still loading.
  Run four times over for stability. The same suite on build 131 fails 29 of
  the 35.
- **250-label render diff vs build 131 — 0 differences** with no walk set.
- With one walk set: banner on 223 of 250, 214 of them red. With two set:
  same 223, 211 red — the three that moved are scans for the second walk. In
  both cases, **0 differences once the banner element is removed**, so the
  feature still touches nothing else.
- Build 115's `liveHtmlLooksSane` accepts the file.

## One note for later

The tally counts in whole milliseconds, so a scan logged in the very same
millisecond as you tap a walk could land on either side of the line. On the
round that's meaningless — it only ever showed up as a flaky test, which is why
the checks above have small waits in them.
