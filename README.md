# Wild App — v1.60 (build 56)

Signed with your own `wild-signing.keystore`, so it installs **over** the app you
have now. Your saved corrections and scan history are kept.

This APK replaces the `index.html` I sent earlier today — **don't upload that
file**, everything in it is baked into this build.

---

## Install it

1. **Get out of the app first** — hold the **BACK** key for 10 seconds, then tap
   **Leave**. (The kiosk fights the installer, and on the current build this is
   also what releases the screen pin.)
2. Open the downloaded `WildApp.apk` and install.
3. Launch it. The bottom-left pill should read **v1.60 · build 56**.

## Then put it in the repo

- Upload the file to `ashkayuk-cmd/wildapp-updates` as **`WildApp.apk`**.
- Set **`version.txt`** to **`56`**.
- Leave `content.json` alone — it says build 54, which this build correctly
  ignores.

---

## What changed

**1. Screen pinning is gone.**
The "Screen is pinned" behaviour came from a native call (`startLockTask`) added
in build 49. That call is removed. The first time you open the new build it also
runs `stopLockTask` once, so if the PDA is still pinned when you install, it
unpins itself.

*Worth knowing:* pinning was the thing that blocked the **recents** button. With
it gone, pressing recents can once again minimise the app for a moment before it
pulls itself back — the behaviour you had on build 48. Tell me if that becomes
annoying and I'll look at other options.

**2. The sleep screen is completely black.**
After 8 minutes idle it used to go 98% black with a sleep face and "Asleep — tap
or pull the trigger". Now there's nothing on it at all. Tap the screen or pull
the trigger to wake, same as before.

**3. "Also possible" walk cards are blue.**
They were amber. They now use the same blue as the main walk. The *"Postcode
doesn't match this address"* warning banner is still amber — that one is a real
warning, so I left it. Say the word if you want it blue too.

**4. The Lancaster Hall label now shows walk 17.**
Your barcode reads `35 CRAVEN TERRACE LANCASTER HALL HO` with postcode `W33EL`,
and the mailing house's own `GL51 9FL` on the end — which is the one the app was
picking up, so it stopped at "Not in W2".

The rescue added in v1.57 was supposed to catch this, but it asked the fuzzy
matcher which address to check, and on a real label the barcode header and
tracking number drown out the address — it picked *"The Cow, 89 Westbourne Park
Road"* and gave up. It now searches for the address that actually shares the most
distinctive words with the label, which lands on Lancaster Hall Hotel.

So the screen now shows **17 LANCASTER** big, with the address under it and a blue
note explaining the walk came from the printed address because the label's
postcode isn't in W2. **Fix it** is on that screen if it's ever wrong.

If a label's address could fit two or three walks, you still get the tappable
list instead — it only answers outright when there's one clear answer.

---

## Checked before delivery

- **Signature**: same certificate as your installed app
  (`2C:3A:BB:7A:B7:00:AF:19…`), v1 + v2 + v3, zip-aligned.
- **Only four files differ** from the build you're running: the code file, the
  manifest (versionCode 49 → 56), and the two app HTML files. The address data,
  the spreadsheet, the libraries and the icon are byte-for-byte identical —
  **24,147 addresses**, Lancaster Hall still on 17 LANCASTER.
- **No `startLockTask` call left anywhere** in the app.
- **101 behaviour tests** run against the app *as packaged inside this APK*,
  including your real barcode, the 8-minute sleep timer fired for real, and
  144 out-of-area labels (none of which invents a W2 walk).
- If you'd already applied an over-the-air update, that cached copy is older than
  this build and is correctly ignored — and after installing, the app won't
  falsely claim another update is waiting.

`WildApp.apk` — 1,082,289 bytes, sha256 `8d149947f43fd664…`
