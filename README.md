# OrchardWalk — Field Phenotyping App

A self-contained, offline-first web app for walking an orchard, finding plants, and capturing geotagged photos and notes tied to each plant's Unique ID — no physical labels required.

Runs entirely in the browser from a single `index.html` file. No installation, no backend server, no database. Works on iPhone (Safari) and desktop (Chrome, Edge, Safari, Firefox).

---

## Quick Start

1. Open `index.html` in a browser (ideally hosted at a real URL — see [Hosting](#hosting--installing-as-an-app) below)
2. Tap **"Use built-in demo data"** to try it immediately, or load your own orchard map
3. Choose **Sequential** or **Search & Capture** mode
4. Walk the orchard, capture photos, add notes, tap Next
5. Tap **End Session** when finished to see a summary and export your data

---

## Loading Your Data

There are three ways to get plant data into the app, all on the first screen:

| Option | What it needs | When to use it |
|---|---|---|
| **Load orchard map (CSV)** | A CSV that already has a `Unique_ID` column | You've already prepared a file with IDs |
| **Create Orchard Map from Excel/CSV** | Any Excel (`.xlsx`/`.xls`) or CSV file, any columns | Your file doesn't have a `Unique_ID` yet — build one in-app |
| **Use built-in demo data** | Nothing | Testing the app or showing someone how it works |

### Required CSV columns (for direct loading)

At minimum, your CSV needs a `Unique_ID` column. The app also recognizes these optional columns if present: `Replication`, `Row`, `Plant_Num`, `Renumber`, `Status`, `Plant_Id`, `Species`, `PI_GMAL`, `Ivno`, `Ivs`, `USDA_GMAL`, `Country`, `Notes`, `seq_order`.

### Building a Unique ID from your own spreadsheet

If your file doesn't have a `Unique_ID` column, use **"Create Orchard Map from Excel/CSV"**:

1. Upload any Excel or CSV — any column names, any layout
2. Tap the columns (in the order you want) that should combine to form each plant's ID — e.g. `Block` + `Row` + `Tree`
3. Choose a separator: underscore, hyphen, space, or none
4. Watch the live preview update as you adjust your selections
5. Review the generated table (first 8 rows shown)
6. Tap **"Use This Map in OrchardWalk"** to load it straight into a walk, or **"Export as CSV Only"** to save the file for later

The app reads Excel files using a small built-in parser — no internet connection is needed, even for `.xlsx` files.

---

## Two Ways to Work: Sequential vs. Search & Capture

### Sequential Mode
Walks through plants in the order they appear in your file — the classic "next plant" workflow.

- **Filter** by Replication, Row, or Status before starting
- **Skip already photographed** plants to resume a partially-finished walk
- Progress bar shows how far through the list you are
- **Skip** moves on without recording anything; **Next** saves your note (and photo, if captured) before advancing

### Search & Capture Mode
Jump straight to any specific plant instead of walking the full list in order.

- Type anything into the search box — a plant ID, variety name, row number, GMAL code, anything — results filter instantly as you type
- Quick filters: **Sampled** / **Unsampled**
- Additional filters appear automatically for columns with a manageable number of distinct values (e.g. Row, Replication, Status, Country, Species)
- Tap any result to open it directly — capture a photo, add notes, and navigate with **Prev / Next** through your search results, or **Back to Search Results** to pick a different plant
- **Every visit to the same plant is recorded separately.** If you photograph the same tree on two different days, both observations are kept — nothing is overwritten. Past observations (with their photos and notes) are shown right on the plant's detail screen.

Both modes have their own **End Session** button and produce the same kind of summary and exportable CSV — they're complementary, not exclusive; switch between them anytime from the Configure Walk screen.

---

## Capturing Photos

Tap **Capture**:
- **iPhone/Android:** opens the native camera directly (rear-facing)
- **Mac/PC:** opens a live webcam preview inside the app with a shutter button

Every photo is automatically named using the plant's Unique ID and the capture timestamp, e.g.:
```
DA008-R1-RW01-P002-KAZ9609-05_2026-06-19T20-55-23.jpg
```
This means any photo can always be traced back to the exact plant and visit, even after the fact.

### Notes timing — does it matter when I type them?

No — the note text box is only read at the moment you tap **Next**, **Skip**, **Prev**, or **End Session**. You can write your note before or after taking the photo; both are saved together as long as you do them during the same visit, before navigating away.

---

## Saving Photos: What Happens and Why

Where a captured photo actually ends up depends on your device and your chosen save preference (the gear icon next to End Session):

| Setting | Behavior | Best for |
|---|---|---|
| **Photos app** (default) | Opens the iOS/Android share sheet; tap "Save Image" to send it to your Photos library | iPhone/Android field use |
| **Files / Downloads** | Downloads silently to your browser's default Downloads folder every time, no prompts | Desktop use, or if you prefer Files over Photos |
| **Persistent Folder** (Chrome/Edge only) | Pick a folder once; every photo, metadata file, and session summary saves directly into it from then on — no prompts, ever again, across sessions and restarts | Desktop fieldwork with Chrome or Edge |

### Why isn't there one universal "pick a folder once" option?

This is a genuine platform limitation, not a gap in the app: **Safari — on both iPhone and Mac — has never implemented the File System Access API** that makes true "remember my folder forever" possible. Only Chrome and Edge support it. Since most iPhone use happens in Safari, the Photos/Files share-based flow is the most practical alternative there — it's the same one-tap pattern every other photography app on iOS uses.

If your save location ever seems to drift back to the wrong setting, open Settings and tap your preferred option again — the choice is stored on your device and will stick from then on.

---

## Field Notes & GPS

- A **Field Notes** box appears for every plant — free text for observations, phenotype data, health notes, anything
- **GPS coordinates** are captured automatically in the background (shown live at the top of each plant's screen) and bundled into the saved record alongside the photo and notes — no separate step needed
- If GPS hasn't acquired a fix yet, the app shows "Waiting for GPS…" and will fill it in as soon as it's available

---

## Ending a Session & Exporting Data

Tap **End Session** (top right, in either mode) to finish and see a summary: plants visited, images captured, images actually saved, notes added, and GPS fixes recorded.

From the summary screen:
- **Export Session Log (CSV)** — downloads a complete CSV linking every plant's Unique ID to its photo filename, timestamp, GPS coordinates, and notes
- **Share Summary** — opens the share sheet to send the CSV via Mail, Messages, AirDrop, etc.
- **Start New Walk** — clears the current session and returns to setup (your Search & Capture observation history is preserved — only the Sequential walk's in-progress data is cleared)

---

## Hosting & Installing as an App

This file works when opened directly (e.g. from Files on iPhone), but for the full experience — installable home-screen icon, reliable camera/GPS access, no "open in Safari" friction — it needs to be hosted at a real web address.

### Free hosting options
- **GitHub Pages** — create a free repository, upload `index.html` (plus `manifest.json`, `sw.js`, and the icon files if you have them), enable Pages in repo settings. You'll get a permanent URL like `https://yourname.github.io/yourrepo/`.
- **Netlify Drop** — drag the folder onto Netlify's drop page for an instant (temporary, unless you sign up free) live URL.

### Installing on iPhone
1. Open your hosted URL in **Safari** (not Mail, not Files — must be Safari itself)
2. Tap the **Share button** then **Add to Home Screen**
3. The app now launches full-screen from its own icon, just like a native app

---

## Offline Behavior

OrchardWalk is built to work with no internet connection at all once loaded:
- The CSV parser, Excel parser, and all app logic are fully embedded — no external libraries are fetched at runtime
- If hosted with its accompanying `manifest.json` and `sw.js` (service worker), the app caches itself for instant offline loading on return visits
- GPS and camera both work offline; only the very first page load (or first install) needs a network connection if hosted remotely

---

## Data Privacy

All data — plant records, photos, notes, GPS coordinates, save preferences — stays entirely on your device. Nothing is uploaded to any server. The app has no backend; it's a single HTML file running in your browser.

---

## Known Limitations

- **Persistent folder selection works only in Chrome or Edge** (desktop or Android) — Safari (iPhone and Mac) does not support this browser feature, by Apple's design, not a limitation of this app
- **iOS requires one tap to confirm each photo save** ("Save Image" in the share sheet) — no website on any browser can skip this, due to Apple's privacy restrictions
- **Excel file parsing** supports standard `.xlsx`/`.xls` exports from Excel, Google Sheets, and Numbers; unusual or heavily macro-laden files may not parse — exporting as CSV is always a safe fallback
- Search results display a maximum of 200 rows at once for performance; narrow your search text to see more specific matches in very large files
