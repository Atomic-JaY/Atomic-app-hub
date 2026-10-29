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
| `life-safety/device.html` | Phone page for one device: `device.html?FE7`. Big compliant / due soon / overdue verdict, every required check with last and next dates and who did it, the last 8 inspections, photos, and the AED prescription certificate. With no ID it lists all 51 devices with a status count. Under 1 MB to load. |
| `life-safety/Life-Safety-PM-Log.xlsx` | The PM log. The Inspection History tab is the record; every Last date on the device tabs is a formula over it. |
| `life-safety/photos/`, `certs/` | Every device photo (`FE1.jpg` close-up, `FE1-loc.jpg` location) and the two AED prescription PDFs, shared by both pages. |
| `life-safety/xlsx.core.min.js` | The spreadsheet reader `device.html` uses. |

### QR labels

Every device has a label with a QR code for `device.html?<ID>`. Anyone who scans it, an
auditor included, sees that device's live status on their own phone with no app and no
login. The labels are permanent, so **don't rename `life-safety/` or `device.html`**:
that would break every label on the floor. If either ever has to move, leave a
`device.html` behind that forwards to the new place.

### Updating the log

1. Walk mode on the map (or typing rows onto the Inspection History tab) produces an
   updated `Life Safety PM Log.xlsx`.
2. Upload it to `life-safety/` as **`Life-Safety-PM-Log.xlsx`**, replacing the old one,
   and commit. GitHub Pages republishes in a minute or two.
3. Until that commit, the map and every QR page show the previous upload. Each status
   page prints when the log was last updated, so a stale upload is visible to anyone
   who scans.

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
