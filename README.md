# OrchardWalk — Setup & Hosting Guide

## Why hosting is required (Issue 1 — the real fix)

The previous versions only worked when opened as a local file (`file://`) on
your iPhone, which is exactly why every workaround was fragile: Mail's preview,
the Files app viewer, and `file://` pages are all locked down by iOS and can't
reliably trigger Safari, the camera, or installation as an app.

**The actual fix is to put these 4 files on a real web address (`https://`).**
Once the app has a URL, every problem changes at once:

- Tapping a link **always** opens Safari — no "Open in Safari" step, ever
- It becomes a true **installable PWA** — Add to Home Screen gives it a real
  icon and makes it open full-screen, no Safari address bar, just like a
  native app
- Camera, GPS, and the Save/Share features all become fully reliable, since
  `https://` origins don't have the restrictions `file://` has

This is not a workaround — it's the standard way every PWA (Starbucks,
Twitter Lite, etc.) achieves a native-app-like experience on iPhone.

---

## Step-by-step: host it for free in ~5 minutes

You don't need to know how to code. Pick **one** of these:

### Option A — Netlify Drop (simplest, no account needed for testing)
1. Go to **app.netlify.com/drop** in any browser
2. Drag the whole `OrchardWalk` folder (all 4 files: `index.html`,
   `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`) onto the page
3. Netlify gives you a live URL like `https://orchardwalk-xyz123.netlify.app`
4. Open that URL on your iPhone in Safari — done

*Note: Netlify Drop links are temporary unless you make a free account. For
permanent use, sign up free at netlify.com, then drag the folder onto your
dashboard — the URL stays live indefinitely.*

### Option B — GitHub Pages (free, permanent, slightly more setup)
1. Create a free account at github.com
2. Create a new repository, upload all 4 files
3. Go to repository **Settings → Pages** → set source to "main" branch
4. GitHub gives you a permanent URL like
   `https://yourname.github.io/orchardwalk/`

### Option C — Your institution's existing web server
If your university or research organization already hosts websites, ask IT
to drop this folder into any web-accessible directory. Any static file host
works — there's no backend, database, or server-side code required.

---

## Once hosted: install on iPhone

1. Open the URL in **Safari** on your iPhone
2. Tap the **Share button** (square with arrow, bottom of screen)
3. Scroll down, tap **"Add to Home Screen"**
4. Tap **Add**

OrchardWalk now has its own icon on your home screen. Opening it from there
launches full-screen with no Safari UI — indistinguishable from a native app
at a glance.

---

## What was fixed in this version

### Issue 1 — Opening on iPhone
**Root cause:** Files delivered via Mail/AirDrop have no URL, so iOS has no
reliable way to know "this should open in Safari" or be installed.
**Fix:** Packaged as a proper PWA (`manifest.json` + service worker +
icons). Once hosted at a URL, install via Add to Home Screen works exactly
like any other PWA (this is how apps like Starbucks' web app work).
**Limitation:** This step requires hosting the files somewhere with a URL —
there is no way to make a locally-saved file behave this way, on any iPhone,
for any app. This is an iOS platform restriction, not specific to this app.

### Issue 2 — Saving photos to Photos library
**Root cause:** iOS Safari deliberately does not allow any website to write
directly to the Photos library without an explicit user tap, for privacy
reasons. This applies to every website, not just this app.
**Fix:** After capture, the app automatically opens the native iOS Share
Sheet, which puts **"Save Image"** as the default top action — typically
one tap. The app now also clearly tracks and displays whether each photo
has actually been saved (a yellow "Not yet saved" badge vs a green "Saved to
Photos" badge), so you always know the true state and can retry per-photo if
a save was missed or cancelled.
**Limitation:** The single tap on "Save Image" in the share sheet cannot be
automated away — this is intentional Apple privacy policy enforced at the OS
level, not a restriction in this app's code.

### Issue 3 — End Session not working
**Root cause:** Found and fixed. The previous code used the browser's native
`confirm()` dialog. In iOS "Add to Home Screen" standalone mode (and
sometimes in regular Safari), `confirm()` can silently fail to fire — especially
right after a text field (the field notes box) was focused, due to a timing
conflict between the keyboard dismissing and the dialog appearing.
**Fix:** Replaced `confirm()` entirely with a custom-built modal dialog that
behaves identically but is implemented in plain HTML/CSS — fully reliable in
every iOS context, standalone or in-tab. Tapping "End Session" now also
explicitly removes keyboard focus first, eliminating the race condition.
Additionally, the session CSV is now auto-generated the moment the summary
screen appears (not just when you tap Export), so data is captured even if
you close the app before exporting.

---

## Files in this package

| File | Purpose |
|---|---|
| `index.html` | The complete app — all screens, logic, styling |
| `manifest.json` | Tells iOS this is an installable app (name, icon, colors) |
| `sw.js` | Service worker — caches the app for instant offline loading |
| `icon-192.png` | Home screen icon (small) |
| `icon-512.png` | Home screen icon (large, for app switcher/splash) |

All 5 files must stay together in the same folder when hosted — they
reference each other by relative path.
