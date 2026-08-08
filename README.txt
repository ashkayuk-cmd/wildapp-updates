WILD APP — v1.75 / build 71
===========================
Content-only release. NO APK, nothing to install.
REPLACES the v1.74 files from earlier — if you haven't uploaded those yet,
just upload these instead.

UPLOAD BOTH FILES to  github.com/ashkayuk-cmd/wildapp-updates  (main):

    index.html          <- the app          (build 71)
    wild_data.json      <- the address data (24,149 rows — unchanged since v1.74)

Edit nothing else. version.txt and content.json are dead files.

Then on the PDA:  tap  ⟳ Update  (bottom-left)  ->  it turns green
                  ->  tap it again to apply.


1. YOUR NEW wild_data.xlsx IS IN  (24,145 -> 24,149 rows)
   * concierge 139 / 149 / 159 Queensway (W2 4BJ) -> 29 HATHERLEY
   * "Concierge 6, Hermitage Street" -> "6, Hermitage Street" (walk 6, unchanged)
   * new "65, Alfred Road, W2 5EU" -> FIRM
   No walk changes, nothing reverted, workbook and data in sync.
   BONUS: "SOUTH CONCIERGE ... QUEENSWAY" now lands on 29 HATHERLEY outright
   instead of 29 with a red "also possible 24 QUEENSWAY" under it.

2. TAP THE WALK -> ALL STREETS ON THAT WALK
   Tap the big walk name on any result: every street on that walk, A-Z, with
   an address count each. Back returns to the scan. Read-only. Won't fire
   while you're mid-correction.

3. VOLUME — FIXED, AND THE BEEP IS ABOUT TWICE AS LOUD
   You said the popup moves but the beeps don't change. That means the native
   side is fine and Android's media volume simply never reaches the app's
   beeps on this device. So:
   * THE VOLUME KEYS NOW SET THE APP'S OWN BEEP LEVEL. The keys and the
     slider on the Sound screen are one and the same setting. A short preview
     tone plays as you press, so you hear the level immediately. The popup now
     reads "Beep volume 55%". Holding a key doesn't stack tones.
   * Device volume at 0 = beeps off, buzz still on, same as the slider at 0.
   * THE BEEP ITSELF IS LOUDER: square wave instead of sine, moved to ~2.6 kHz
     where a small speaker actually projects, and it now uses the headroom the
     old settings left unspent (peak 0.29 -> 0.50 at the default). Modelled
     through a speaker + ear-sensitivity curve: about +8 dB at the default
     setting and +7 to +12 dB across the range — roughly twice as loud.
   * The four outcomes still sound different from each other, and FIRM still
     gets its low tail. They're all just higher and harder now.
   If it's STILL not loud enough, say so — 100% on the slider has more in it
   than the old build's maximum, so try the keys at full first.

4. GITHUB UPLOAD: PIN + THE TOKEN STOPS DISAPPEARING
   * Upload now asks for PIN 1984, on its own keypad (a text box summons
     Android's keyboard, which the kiosk fights).
   * Found why you kept retyping it: the token was ONLY saved after a
     SUCCESSFUL upload. It's now saved the moment you tap Upload, there's a
     "Save token on this device" button, and a rejected token gets cleared
     with a message instead of failing silently forever.
   * The token is NOT baked in yet — on purpose. See below.

5. Carried forward from v1.72 (never uploaded): the "tap to copy" tracking
   number chip is gone from all three result cards.


THE TOKEN — 60 SECONDS OF YOUR TIME AND I'LL BAKE IT IN
-------------------------------------------------------
I checked what your token can do before embedding it. It's scoped to just
this repo, but it has administration and contents access, not only Issues.
index.html lives in a PUBLIC repo, so anything baked into it is readable by
anyone — and with contents access, whoever picks it up could replace
WildApp.apk in your own update channel. GitHub's secret scanner would also
spot it and auto-revoke it, so it would stop working anyway.

  GitHub -> Settings -> Developer settings -> Personal access tokens ->
  Fine-grained -> your token -> Repository permissions:
      Issues:         Read and write
      Contents:       No access
      Administration: No access

Tell me when that's done and I'll ship it pre-loaded — the plumbing is
already in the file, it's a one-line change. Worst case then is someone
opening issues on your repo, which you can delete.


STAMPS   code 71 · data 70 · rows 24149 · hash e71e95e7 · apk 62
TESTS    78 green (37 + 11 PIN + 8 gates + 22 volume), plus 274 real labels
         rendered through v1.74 and v1.75: 0 differences (audio only).
