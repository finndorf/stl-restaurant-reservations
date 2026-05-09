# STL Restaurant Reservations

Automated Friday night reservation checker for 6 St. Louis restaurants. Runs as scheduled remote Claude Code routines that scan availability, write results to `latest-results.json`, and email Cam when openings appear. Results are also rendered in the [cam-dashboard](https://github.com/finndorf/cam-dashboard) `/dinner` page.

## Restaurants Tracked
| Restaurant | Platform | Reservation Release |
|---|---|---|
| Wright's Tavern | Resy | 1st of month, 12:01 AM Central |
| The Mainlander | Resy | 1st of month, noon Central |
| Robin Restaurant | Resy | Rolling 60-day window |
| Louie on DeMun | Resy | Standard rolling |
| The Crossing | OpenTable | Standard rolling |
| Acero | OpenTable | Standard rolling |

## Automated Routines

| Routine | Schedule (Central) | What it does |
|---|---|---|
| Weekly Friday scan | Mon 8:00 AM | All 6 venues, next 4 Fridays |
| Wright's drop | 1st 12:01 AM | Wright's Tavern only, newly released month |
| Mainlander drop | 1st 12:00 PM | The Mainlander only, newly released month |

Each routine writes `latest-results.json` to the repo root and emails `cam.conway@gmail.com` if anything is open.

## Files
- `stl-friday-reservation-check/SKILL.md` — weekly Friday scan logic
- `stl-monthly-reservation-drop/SKILL.md` — monthly drop logic (parameterized by venue in routine prompt)
- `latest-results.json` — most recent scan output (rewritten on each run)

## Note on auto-booking
The routines do **not** book reservations. Resy and OpenTable forbid automation, and a flagged account would lose card-on-file privileges. The system surfaces openings + booking deep-links (e.g. `?date=YYYY-MM-DD&seats=2`) — Cam books in one tap from the email or dashboard.
