# Wild App — fixed

## Root cause (finally confirmed, not guessed)
`MainActivity.requestAdmin()` had a real bytecode bug: the method declared
only 8 registers with the implicit `this` pinned to the last one (`v7`). A
64-bit constant a few lines later (`const-wide/32 v6, 0x1d4c0`) legitimately
needs a *register pair* — `v6` **and** `v7` — to hold its value. That pair
silently overwrote `this` right before it was used again, and the on-device
verifier correctly refused to run the method at all once it noticed.

That's why nothing could ever catch it: a verifier rejection kills the whole
class before any of its code — constructor, `attachBaseContext`, `onCreate`,
any try/catch inside them — gets a chance to run. It also explains why it hit
100% of the time on this device with zero exceptions logged anywhere, why an
old build without this method (or with different register allocation) still
worked, and why the crash followed the app through every reset, cert change,
and device wipe.

**Fixed** by widening the register window on that one method so nothing
collides with `this`. Scanned every other method in the app for the same
pattern — nothing else was affected.

## What's still in this build
- The on-screen crash display (from earlier in this debugging session) is
  still wrapped around `onCreate`'s WebView setup, as a safety net — if
  anything ever does throw a normal Java exception there again, you'll see it
  on screen instead of a silent "has stopped."
- The crash logger now also installs at the earliest possible point
  (`attachBaseContext`), before it did previously.

Neither of these change normal behaviour — they only matter if something
goes wrong again.

## What this doesn't touch
Nothing about resolver logic, walk data, PIN handling, or OTA update flow
changed. Same version (1.15 / build 170), same signing cert — installs as a
normal update.
