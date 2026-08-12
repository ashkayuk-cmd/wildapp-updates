Wild App — v1.79 (build 75)
============================
This is an OTA CODE update (index.html only) — no APK, no install.
Upload: index.html
Data: unchanged (24,149 rows, hash e71e95e7).

WHAT'S NEW
----------
- ⚙️ Options button added next to the version number (bottom-left, works
  from any tab). Opens a small menu:
    🔊 Sound  — same screen as before
    🗣️ Voice  — same screen as before
  More settings can be added to this menu later as one more row each.

IMPORTANT — this build also carries forward the reverts from build 74
------------------------------------------------------------------
The repo's index.html was still on build 73 (v1.77), which is BEFORE the
two reverts you asked for on 11 Aug (tracking numbers registering again,
sound settings back to how they were). Building on top of the stale repo
copy would have silently undone both fixes, so this build re-applies them
on top of the new Options button:
  - 1D tracking-number barcodes register again (buzz, tone, "No postcode
    found") instead of being silently ignored.
  - Sound is back to the original beep shape/volume curve from before the
    11 Aug change.

Release: on the PDA, tap ⟳ Update → when it goes green → tap again.
