# Atomic Industries — Plant Apps

Internal tools for the Shelby Township plant. Plain HTML, no build step, no framework,
no server-side anything — every app here is a folder with an `index.html` in it, served
straight off GitHub Pages.

**Live site:** https://USERNAME.github.io/REPO/
*(replace `USERNAME` and `REPO` after the first deploy)*

## What's here

| App | Folder | What it does |
| --- | --- | --- |
| Life Safety PM Tracker | [`life-safety-pm/`](life-safety-pm/) | Interactive plant map of 21 fire extinguishers, 23 emergency lights and 4 eye wash stations, with the PM interval each one owes and photos of every unit. |

## Life Safety PM Tracker

Open `life-safety-pm/` and the tracker reads `Life-Safety-PM-Log.xlsx` from that same
folder by itself — nobody needs a copy of the workbook on their own machine. Markers are
green when compliant, amber inside the due-soon window, flashing red when overdue, and
grey when no date has been logged yet. Click one for its photos and its full list of
checks.

Intervals are code-driven, not guesses:

- **Fire extinguishers** — monthly inspection, annual maintenance, internal examination
  and hydrostatic test per **NFPA 10**. Agent type drives the last two, which is why the
  five CO2 units (FE2, FE4–FE7) run on a 5-year hydro and the sixteen dry chemical units
  on 12-year.
- **Emergency lights** — 30-second monthly function test and the 90-minute annual test
  per **NFPA 101 §7.9.3**.
- **Eye wash stations** — weekly activation per **ANSI/ISEA Z358.1**, with the annual
  inspection, and **OSHA 1910.151(c)** as the underlying requirement.

### Updating the log

1. Edit `life-safety-pm/Life-Safety-PM-Log.xlsx` — type the date a check was done into
   the `Last …` column. Every `Next …`, `Days Remaining` and `Status` cell recalculates
   itself.
2. Commit the workbook.
3. The site picks it up on the next page load. Nothing to rebuild.

Only the `Last …` columns and `Notes` are meant to be typed in. The rest are formulas.

### Photos

`life-safety-pm/photos/` holds two shots per device — `FE1.jpg` for the unit and
`FE1-loc.jpg` for the wall it sits on, and the same pattern for `EL*` and `EW*`. The full
naming rules are in `photos/READ ME - photo naming.txt`.

**Replacing** a photo needs nothing but the commit: same filename, and the site picks up
the new image. **Adding a filename that isn't there yet** (an extra angle, or a device
added later) also needs one line in the `PHOTO_MANIFEST` object near the top of
`life-safety-pm/index.html`, e.g. `"FE1-2":"FE1-2.jpg"`. That list is what lets the page
request each photo exactly once instead of guessing at file extensions — without it, a
page load costs several hundred failed requests.

## Two open compliance items

These are findings, not bugs, and both need a person to decide:

- **EW2 is squeeze bottles** at the hi-lo battery chargers, signed as an emergency
  eyewash station. Under Z358.1 bottles are supplemental only — they don't deliver the
  15-minute flush a battery-acid splash needs. That location calls for a plumbed or
  self-contained unit.
- **All four eye wash stations read overdue.** Z358.1 asks for a *weekly* activation and
  the log is set to weekly, so they've been overdue since the 07/23/2026 baseline. The
  fix is a weekly walk, not a longer interval.

## Notes

- `.nojekyll` at the root turns off Jekyll processing so Pages serves these files as-is.
- The tracker is one self-contained HTML file: the plan drawing is an inlined image and
  the spreadsheet reader is vendored in. Its only external reads are the workbook and the
  photos, both from its own folder.
