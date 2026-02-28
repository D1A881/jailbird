# JailbirdWEB

A single-file browser app for browsing the Lewis & Clark County, Montana jail roster and Helena Municipal Court arrest warrant list. No server required — open `jail.html` directly in any modern browser.

---

## Features

### Jail Roster (All tab)
- Loads the inmate roster PDF from Lewis & Clark County Sheriff's Office
- Parses and displays all records in a sortable table
- Columns: Name, Age, Sex, Booking Date, Days Elapsed, Booking #, Search
- Live search across name, charges, and booking number
- Click **Open Roster PDF** in the header to load a local PDF instead of fetching from the network

### PDF Caching
- On first load the PDF is downloaded and stored locally in **IndexedDB**
- Subsequent visits load instantly from the local cache (no network request)
- Cache expires after **1 hour**, at which point a fresh copy is fetched automatically
- If the network is unavailable, the stale cached copy is used as a fallback
- Loading a local file via **Open Roster PDF** updates the cache

### Fetch fallback chain
The app tries the following in order until one succeeds:
1. `XMLHttpRequest` — direct to county server
2. `fetch` — direct to county server
3. `XMLHttpRequest` — via [corsproxy.io](https://corsproxy.io)
4. `fetch` — via [corsproxy.io](https://corsproxy.io)
5. Stale IndexedDB cache (if available)

### Search Icons
Each inmate row includes six one-click search icons:

| Icon | Service |
|------|---------|
| ![Facebook](https://img.shields.io/badge/-Facebook-1877F2?style=flat-square&logo=facebook&logoColor=white) | Facebook People Search |
| ![Google](https://img.shields.io/badge/-Google-4285F4?style=flat-square&logo=google&logoColor=white) | Google |
| ![DuckDuckGo](https://img.shields.io/badge/-DuckDuckGo-DE5833?style=flat-square&logo=duckduckgo&logoColor=white) | DuckDuckGo |
| ![Yandex](https://img.shields.io/badge/-Yandex-FC3F1D?style=flat-square) | Yandex |
| ![Bing](https://img.shields.io/badge/-Bing-008373?style=flat-square&logo=microsoftbing&logoColor=white) | Bing |
| 🌐 | InmateAid |

**Quoted / Plus toggle** (header button) switches the search query format between:
- **Plus** (default) — `Firstname+Lastname`
- **Quoted** — `"Firstname Lastname"`

The toggle applies to all six icons simultaneously, including the Arrest Warrants tab.

### Overview tab
Statistics summary for the current roster:
- Total inmates, male/female counts, warrant count, drug-related, violent, average age
- Hold type breakdown bar chart (Warrant, Charge, Sentenced, DOC Hold, P&P)
- Top charge category bar chart (Partner/Family Assault, Drug Possession, Burglary/Theft, etc.)

### Arrest Warrants tab
- Fetches the Helena Municipal Court arrest warrant list from [helenamt.gov](https://www.helenamt.gov/Departments/Municipal-Court/Arrest-Warrants-List)
- Displays one name per line with the same six search icons
- Live filter input to search within the list
- **Open Warrant HTML** button — load a locally saved copy of the warrant page instead of fetching from the network
- List is loaded lazily on first tab visit and cached for the session

### County Websites tab
Links to all 57 Montana county government websites in a responsive grid.

### Links tab
Quick links to:
- [Helena Arrest Warrants List](https://www.helenamt.gov/Departments/Municipal-Court/Arrest-Warrants-List)
- [LC County Inmate Roster PDF](https://www.lccountymt.gov/files/assets/county/v/0001/sheriff/documents/jail-roster.pdf)

---

## Usage

1. Download `jail.html`
2. Open it in any modern browser (Chrome, Firefox, Edge, Safari)
3. The roster loads automatically — no installation, no server needed

To load a saved local copy of the PDF:
- Click **Open Roster PDF** in the header and select the file

To load a saved local copy of the warrant list:
- Go to the **Arrest Warrants** tab
- Click **Open Warrant HTML** and select a locally saved `.html` copy of the Helena warrant page

---

## Data Sources

| Source | URL |
|--------|-----|
| LC County Jail Roster PDF | `https://www.lccountymt.gov/files/assets/county/v/183/sheriff/documents/pinmates.pdf` |
| Helena Municipal Court Warrant List | `https://www.helenamt.gov/Departments/Municipal-Court/Arrest-Warrants-List` |

Data is public information published by Lewis & Clark County Sheriff's Office and the City of Helena Municipal Court.

---

## Technical Details

- **Single file** — all HTML, CSS, and JavaScript in one `jail.html`
- **No build step, no dependencies to install**
- **PDF parsing** — [PDF.js 3.11](https://mozilla.github.io/pdf.js/) via CDN
- **Fonts** — DM Mono + DM Sans via Google Fonts
- **Storage** — IndexedDB for PDF caching; no cookies, no localStorage
- **CORS proxy** — [corsproxy.io](https://corsproxy.io) as fallback if the county server blocks direct browser requests

---

## Notes

- The county PDF URL may change over time. If the roster fails to load, check the [LC County Sheriff page](https://www.lccountymt.gov/sheriff) for the current link and load the PDF manually using **Open Roster PDF**.
- The warrant list is updated periodically by Helena Municipal Court; the date is shown in the tab after loading.
- This app is a read-only viewer. It does not submit any data or communicate with any server other than the county PDF host and corsproxy.io.
