WILD APP — build 195 (v4.80)   OTA release
==========================================

UPLOAD: index.html only.
No data file, no APK reinstall.
Includes everything from build 194 (Last updated panel, lighter blue buttons).


1. BACK BUTTON NOW RETURNS TO OPTIONS — FIXED
---------------------------------------------
Every Options sub-screen leaves a mark on a "browsing trail", and the BACK key
walks that trail one step at a time. Two screens were never registering:

  - App version
  - Walk corrections (the manage screen)

With no mark on the trail, BACK fell straight through to the code at the end of
the handler that collapses to the scan screen - which is exactly the "goes to
main app home page" you saw. Every other sub-screen (Walks, Recent scans,
Shared streets, Sound, Splits, Today's walk, Back button menu, Brightness,
Notification pull-down) was already registering, which is why only some of them
misbehaved.

Both now register, so:

  Options > App version                        > BACK  = Options
  Options > Walk corrections                   > BACK  = Options   (was broken)
  Options > App version > Walk corrections     > BACK  = App version, then Options
  Options > App version > Notification pull-down > BACK = App version, then Options

There was already a special case in the BACK handler for reaching Walk
corrections FROM App version, but nothing for reaching it from Settings - that
second route was the one dropping you home. It is on the trail now, so it
retraces whichever way you actually opened it. The old special case is left in
as belt-and-braces.

No PIN was added or removed. The trail only ever replays a screen you already
passed the PIN to reach, and it is wiped when you go home, so there is no stale
entry left behind to back into later.


2. NOTIFICATION PULL-DOWN "MX said: FAILURE"
--------------------------------------------
WHAT I FOUND: this is not a fault in the app's own code. I disassembled the
installed APK and traced it:

  WildKiosk.pullDown(mode)
    -> builds  <wap-provisioningdoc>
                 <characteristic version="10.1" type="UiMgr">
                   <parm name="NotificationPullDown" value="1|2"/>
                 </characteristic>
               </wap-provisioningdoc>
    -> ZebraScanner.applyMx()  -> EMDK processProfile()
    -> returns MX's own status word

"FAILURE" is MX's status code coming straight back - the device rejected the
profile. The most likely reason: the XML asks for UiMgr version "10.1", and
this is a TC56 on Android 8, whose MX is older than that. Zebra and third-party
working examples of NotificationPullDown use version 5.1. Asking for a newer
version than the device supports is a standard cause of a flat FAILURE.

WHY I HAVE NOT FIXED IT IN THIS RELEASE: that version string is baked into the
dex, and pullDown() only accepts an integer - there is no way to pass different
XML from the web layer. It needs a native APK rebuild, and I cannot test MX
behaviour without the device in hand.

WHAT THIS RELEASE DOES DO: the screen no longer just prints "MX said: FAILURE".
It now explains that the device refused it, that it is not an app fault, and
that StageNow is the way to change it meanwhile. It also stops wiping the
message after 0.9s - the screen only redraws on success now, so you can
actually read the refusal. All the other things MX can return (EMDK not open,
no PROFILE feature, ERR ..., no bridge) get their own plain-English line.

The saved state is untouched on a refusal, so it correctly still reads
"not set" rather than pretending it worked.

TO ACTUALLY FIX IT I would rebuild the APK and either drop the version stamp to
5.1, or better, have pullDown try a ladder of MX versions and report which one
the device accepts - that self-diagnoses instead of me guessing at your PDA's
MX level. Say the word and I will build it.


STAMPS
------
  THIS_BUILD_NUM   195
  BUILD_NAME       v4.80
  REPO_DATA_BUILD  131        (unchanged)
  REPO_DATA_HASH   a5195d2e   (unchanged)
  REPO_DATA_ROWS   24102      (unchanged)
  REPO_APK_BUILD   193        (unchanged - no new APK)


TESTED
------
  - all 6 inline script blocks parse clean
  - 13 back-trail tests using the real handNav/handNavBack pulled out of this
    file, covering both broken routes, the nested routes, revisiting a screen
    (trail truncates rather than growing), a screen re-rendering itself (no
    double entries), and the end of the trail still falling through to the old
    collapse-to-home
  - 14 tests on the MX explainer against every string the disassembled dex can
    actually return, including HTML-injection escaping
  - confirmed all 12 Options sub-screens now register on the trail
  - confirmed the build 194 work (Last updated panel, #10778F buttons) survived
