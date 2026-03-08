<img width="550" height="550" alt="image" src="https://github.com/user-attachments/assets/fbfc6d00-2128-4449-a72c-8c14fc2b28be" />

---

A single-file browser app for browsing the Lewis & Clark County jail roster and Helena Municipal Court arrest warrant list. No server, no installation. Just open `jail.html` directly in any modern browser.

## Quick Start

1. Download `jail.html`
2. Open it in Chrome, Firefox, Edge, or Safari
3. The roster PDF loads automatically on first visit and is cached locally for subsequent visits

## Downloading Data Files Manually

If the automatic fetch fails (e.g. the county server is blocking your browser, or you're offline), you can download the data files yourself and load them directly into the app.

### Inmate Roster PDF

1. Open the **Links** tab in the app
2. Click **Inmate Roster PDF** This opens:
   ```
   https://www.lccountymt.gov/files/assets/county/v/0001/sheriff/documents/jail-roster.pdf
   ```
3. Your browser will either display or download the PDF. If it displays, press `Ctrl+S` / `Cmd+S` to save it, or right-click → **Save as…**
4. Save the file anywhere on your computer (e.g. `Downloads/jail-roster.pdf`)
5. Back in the app, click **Open Roster PDF** (top-right of the header)
6. Select the file you just saved
7. The roster will load immediately and the file will be saved to the local cache for future visits

### Arrest Warrant List

1. Open the **Links** tab in the app
2. Click **Arrest Warrants** This opens:
   ```
   https://www.helenamt.gov/Departments/Municipal-Court/Arrest-Warrants-List
   ```
3. Save the page as an HTML file:
   **Chrome / Edge:** `Ctrl+S` / `Cmd+S` → choose **Webpage, Single File** (`.mhtml`) or **Webpage, HTML Only** (`.html`) → save
   **Firefox:** `Ctrl+S` / `Cmd+S` → choose **Web Page, HTML only** → save
   **Safari:** `File → Save As` → **Page Source**
4. Save the file anywhere (e.g. `Downloads/arrest-warrants.html`)
5. Back in the app, open the **Arrest Warrants** tab
6. Click **Open Warrant HTML**
7. Select the file you just saved
8. The warrant list will populate with all names and search icons

> **Tip:** Both files can be re-loaded at any time using the same buttons. Loading a new PDF replaces the cached copy for future visits.

## Features

### All tab (Jail Roster)
    Parses and displays all inmate records in a sortable table
    Columns: Name, Age, Sex, Booking Date, Days Elapsed, Booking #, Search icons
    Live search across name, charges, and booking number
    If the PDF fails to load, all other tabs remain fully functional and an error message is shown only inside this tab

### Overview tab
    Total inmates, male/female split, warrant count, drug-related and violent offense counts, average age
    Hold type breakdown and top charge category charts

### Arrest Warrants tab
    Fetches the Helena Municipal Court warrant list live, or load a saved HTML file with **Open Warrant HTML**
    One name per row with six search icons
    Live filter input

### County Websites tab
    Links to all 57 Montana county government websites

### Links tab
    Direct links to the source data files (see [Downloading Data Files Manually](#downloading-data-files-manually) above)

---

## Search Icons

Each name row has six one-click search icons. The **Plus / Quoted** toggle in the header switches the query format:

| Mode | Format |
|------|--------|
| **Plus** (default) | `Firstname+Lastname` |
| **Quoted** | `"Firstname Lastname"` |

Icons: Facebook · Google · DuckDuckGo · Yandex · Bing · InmateAid

---

## PDF Caching

    First load: PDF is downloaded and stored in **IndexedDB** (browser's on-disk storage)
    Subsequent visits: loaded from cache instantly. No network request
    Cache expires after **1 hour**, triggering a background refresh
    If the network fails and a cached copy exists, the stale copy is used automatically
    Loading a file via **Open Roster PDF** updates the cache

### Fetch fallback chain

When fetching the PDF automatically, the app tries these in order:

1. `XMLHttpRequest` direct to county server
2. `fetch` direct to county server
3. `XMLHttpRequest` via [corsproxy.io](https://corsproxy.io)
4. `fetch` via [corsproxy.io](https://corsproxy.io)
5. Stale IndexedDB cache (if available)
6. Inline error in the All tab (all other tabs remain usable)

---

## Data Sources

| Data | URL |
|------|-----|
| Inmate Roster PDF | `https://www.lccountymt.gov/files/assets/county/v/0001/sheriff/documents/jail-roster.pdf` |
| Arrest Warrant List | `https://www.helenamt.gov/Departments/Municipal-Court/Arrest-Warrants-List` |

Data is public information published by Lewis & Clark County Sheriff's Office and the City of Helena Municipal Court.

---

## Technical Details

    **Single file** All HTML, CSS, and JavaScript in `jail.html`
    **No build step, no dependencies to install**
    **PDF parsing** [PDF.js 3.11](https://mozilla.github.io/pdf.js/) via CDN
    **Fonts** DM Mono + DM Sans via Google Fonts
    **Storage** IndexedDB for PDF caching; no cookies, no `localStorage`
    **CORS proxy** [corsproxy.io](https://corsproxy.io) as fallback for cross-origin fetch
