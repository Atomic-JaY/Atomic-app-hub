# Atomic Industries — Plant Apps

Internal tools for the Shelby Township plant. Plain HTML, no build step, no framework,
nothing server-side — served straight off GitHub Pages.

**Live site:** https://atomic-jay.github.io/Atomic-app-hub/

## What's here

| App | What it does |
| --- | --- |
| Life Safety PM Tracker (`index.html`) | Interactive plant map of 21 fire extinguishers, 23 emergency lights and 4 eye wash stations, with the PM interval each one owes and photos of every unit. |

## Life Safety PM Tracker

Open the live site and the tracker reads `Life-Safety-PM-Log.xlsx` out of this repo by
itself — nobody needs a copy of the workbook on their own machine. Markers are green when
compliant, amber inside the due-soon window, flashing red when overdue, and grey when no
date has been logged yet. Click one for its photos and its full list of checks.

All 96 photos are built into `index.html`, so the page has no folder to keep alongside it
and nothing to download but itself.

Intervals are code-driven, not guesses:

- **Fire extinguishers** — monthly inspection, annual maintenance, internal examination
  and hydrostatic test per **NFPA 10**. Agent type drives the last two, which is why the
  five CO2 units (FE2, FE4–FE7) run on a 5-year hydro and the sixteen dry chemical units
  on 12-year. The internal-exam and hydro clocks start from the cylinder date of
  manufacture (ZX26 shell stamps, 2026 collar tags), entered as 01/01/2026 — the earliest
  the stamp allows, so every date it produces is the conservative one.
- **Emergency lights** — 30-second monthly function test and the 90-minute annual test
  per **NFPA 101 §7.9.3**.
- **Eye wash stations** — weekly activation per **ANSI/ISEA Z358.1**, with the annual
  inspection, and **OSHA 1910.151(c)** as the underlying requirement.

### Updating the log

1. Edit `Life-Safety-PM-Log.xlsx` — type the date a check was done into the `Last …`
   column. Every `Next …`, `Days Remaining` and `Status` cell recalculates itself.
2. Commit the workbook.
3. The site picks it up on the next page load. Nothing to rebuild.

Only the `Last …` columns and `Notes` are meant to be typed in. The rest are formulas.

### Changing photos

Because the photos live inside `index.html`, swapping one means rebuilding that file
rather than committing a JPG. If you'd rather have them swappable, the folder version of
the tracker does that: it reads `photos/FE1.jpg`, `photos/FE1-loc.jpg` and so on from a
`photos` folder next to it, so replacing a photo is just a commit with the same filename.
That version is a 1 MB `index.html` plus a 97-file `photos` folder, which is more to
upload but easier to maintain month to month.

## Two open compliance items

Findings, not bugs, and both need a person to decide:

- **EW2 is squeeze bottles** at the hi-lo battery chargers, signed as an emergency
  eyewash station. Under Z358.1 bottles are supplemental only — they don't deliver the
  15-minute flush a battery-acid splash needs. That location calls for a plumbed or
  self-contained unit.
- **All four eye wash stations read overdue.** Z358.1 asks for a *weekly* activation and
  the log is set to weekly, so they've been overdue since the 07/23/2026 baseline. The
  fix is a weekly walk, not a longer interval.

## Adding more apps later

Put each new app in its own folder with its own `index.html` — `press-checks/`,
`tooling/`, whatever — and it publishes at `…github.io/Atomic-app-hub/press-checks/`
with no other setup. This top-level `index.html` stays the life safety tracker until you
decide to make it a landing page instead.
