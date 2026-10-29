# Atomic Industries — Plant Apps

Internal tools for the Shelby Township plant. Plain HTML, no build step, no framework,
nothing server-side — served straight off GitHub Pages.

**Live site:** https://atomic-jay.github.io/Atomic-app-hub/ (a landing page linking each app)

## What's here

| App | What it does |
| --- | --- |
| Life Safety PM Tracker (`life-safety/`) | Interactive plant map of 21 fire extinguishers, 23 emergency lights, 5 eye wash stations and 2 AEDs, with every PM check each one owes, walk mode for logging inspections, and a QR label on every device that opens its live status. |
| SDS Library (`sds/`) | Every Safety Data Sheet in the plant, searchable from a phone. Replaces SDS Manager. Prints container labels, the Right to Know notice, the HazCom chemical inventory and the SDS station QR poster. |

## SDS Library

**Live:** https://atomic-jay.github.io/Atomic-app-hub/sds/

Works the same way as the PM tracker: the page reads `sds/SDS-Register.xlsx` out of this
repo, and each row points at a PDF in `sds/pdf/`. No accounts, no per-chemical fees.

| Page | Use |
| --- | --- |
| Library | Search by product, supplier or hazard; filter by area, signal word or pictogram. Each SDS shows pictograms, hazards, PPE, first aid, fire and spill steps, and opens the full PDF. |
| Container label | From any SDS: a 4×6 or 4×2 workplace label for spray bottles and pails (1910.1200(f)(6)). |
| New & revised | SDSs added in the last 30 days, laid out as the MIOSHA Right to Know notice. Print and post within 5 working days. |
| Inventory | The hazardous chemical list for the written HazCom program, grouped by area. |
| SDS poster | Letter-size QR poster for each SDS station. |
| Needs review | Everything with a Review Flag in the register: outdated, foreign-language, incomplete or duplicate SDSs. |

### Adding a chemical

1. Add a row to the `Register` tab of `SDS-Register.xlsx` (the workbook's first tab walks through it).
   Give it a new ID and set `PDF File` to the file name, e.g. `4000001.pdf`.
2. Upload the PDF to `sds/pdf/` and commit.
3. Upload the workbook to `sds/` over the old one and commit. The site updates in about a minute.

To retire a product, set `Status` to `Archived`. Don't delete the row or the PDF.

### Where the data came from

The 98 SDSs and their summaries were exported from SDS Manager on 10/22/2026. Signal
words, hazard codes and phone numbers that SDS Manager missed were checked against the
PDFs. The PDF is always the official SDS; the summaries are a quick reference.

Keep a printed or USB backup at the SDS station for when the network is down.

## Life Safety PM Tracker

**Map:** https://atomic-jay.github.io/Atomic-app-hub/life-safety/
**Device status (what the QR labels open):** https://atomic-jay.github.io/Atomic-app-hub/life-safety/device.html

Both pages read `life-safety/Life-Safety-PM-Log.xlsx` out of this repo on every load, so
the workbook in the repo **is** the record. Nobody needs a login and nobody needs a copy.

| File | What it is |
| --- | --- |
| `life-safety/index.html` | The plant map, about 2.5 MB. Photos load from `photos/` and the AED certificates from `certs/`, so swapping a photo is a commit with the same filename. `index.html#FE7` opens straight onto FE7. (The copy on the X: share is the single-file version with everything built in.) |
| `life-safety/device.html` | Phone page for one device: `device.html?FE7`. Big compliant / due soon / overdue verdict, every required check with last and next dates and who did it, the last 8 inspections, photos, and the AED prescription certificate. Opened from the device's own label, it also logs that device's routine check. With no ID it lists all 51 devices with a status count and the current walk. Under 1 MB to load. |
| `life-safety/Life-Safety-PM-Log.xlsx` | The PM log. The Inspection History tab is the record; every Last date on the device tabs is a formula over it. |
| `life-safety/photos/`, `certs/` | Every device photo (`FE1.jpg` close-up, `FE1-loc.jpg` location) and the two AED prescription PDFs, shared by both pages. |
| `life-safety/xlsx.core.min.js` | The spreadsheet reader `device.html` uses. |

### QR labels and logging an inspection

Every device has a label with a QR code. Scanning it opens `device.html?<ID>` with
that device's live status: no app, no login, so an auditor can check any device.

Each label also carries a private code. Only a scan of the real label unlocks the
**Pass / Fail** buttons for that device; typing the page address gives the read-only
view. The site holds only scrambled (SHA-256) versions of the codes, and the codes
themselves are never in this repo. **Keep the label PDF off GitHub**, since a copy
of a label works like the label. The labels are permanent, so don't rename
`life-safety/` or `device.html`.

**A walk:** scan a label, check the device, and tap Pass or Fail. A Fail needs a
note. Your name is asked once per phone. Each entry records the clock time and
"QR scan". There is no bulk pass: every device needs its own scan. The plant map's
🚶 Walk mode shows what the walk has covered but cannot log anything itself.
**Save to log** (on any status page or the map) downloads the updated
`Life-Safety-PM-Log.xlsx`.

**Publishing it:** upload that file to `life-safety/` here (Add file → Upload files →
Commit), replacing the old one. GitHub Pages republishes in a minute or two. Until
then, only the inspector's phone shows the walk; every status page prints when the
log was last updated.

### The record

- The **Inspection History** tab is the record: one row per inspection, never
  overwritten. Its **Logged at** column holds the time and method of scanned entries.
- The tab is protected in Excel to stop accidental edits. Vendor and annual work
  (UniFirst annuals, hydro tests, 90-minute light tests, eye wash annuals) is added
  by hand: Review → Unprotect Sheet, add the rows, protect it again.
- Every upload here is kept in the commit history with its date and the exact rows
  that changed, so any edit to an old inspection shows.
- A Fail stays on the record and does not reset the due date.

Intervals are code-driven (NFPA 10, NFPA 101 §7.9.3, ANSI/ISEA Z358.1, ZOLL for the
AEDs) and live on the workbook's Intervals tab. Change a number there and every due
date moves with it.

### Public by design

This repo is public, so the map, the photos, the floor plan, the workbook (notes
included) and the AED certificates can be opened by anyone who has the link. That
is what lets an auditor scan a label without an account. Keep anything that
shouldn't be public out of the workbook's Notes columns.

## Adding more apps later

Put each new app in its own folder with its own `index.html`, like `sds/` and
`life-safety/`. It publishes at `…github.io/Atomic-app-hub/<folder>/` with no other
setup. Add a tile for it to the top-level `index.html`.
