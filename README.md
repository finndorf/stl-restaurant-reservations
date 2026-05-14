# STL Restaurant Reservations

Automated Wed–Sun dinner reservation checker for 6 St. Louis restaurants. Runs as scheduled remote Claude Code routines that scan availability, write results to `latest-results.json`, and email Cam when openings appear. Results are also rendered in the [cam-dashboard](https://github.com/finndorf/cam-dashboard) `/dinner` page.

## Restaurants Tracked

| Restaurant | Platform | Reservation Release |
|---|---|---|
| Wright's Tavern | Resy | 1st of month, 12:01 AM Central |
| The Mainlander | Resy | 1st of month, noon Central |
| Robin Restaurant | Resy | Rolling 60-day window |
| Louie on DeMun | Resy | Standard rolling |
| The Crossing | OpenTable | Standard rolling |
| Acero | OpenTable | Standard rolling |

## Search Criteria

- **Nights:** Wednesday through Sunday
- **Lookahead:** Next 14 days (weekly scans) or full upcoming month (monthly drops)
- **Party size:** 2 first, then 4
- **Time window:** 6:00–7:30 PM for all restaurants except The Mainlander
- **Mainlander:** 6:00 PM fixed seating only (operates fixed seatings at 6:00 PM and 8:30 PM; Wed–Sat)

## Automated Routines

| Routine | Schedule (Central) | What it does |
|---|---|---|
| Weekly Mon scan | Mon 8:00 AM | All 6 venues, next 14 days Wed–Sun |
| Weekly Thu scan | Thu 8:00 AM | All 6 venues, next 14 days Wed–Sun |
| Wright's drop | 1st of month, 7:00 AM | Wright's Tavern only, newly released month |
| Mainlander drop | 1st of month, noon | The Mainlander only, newly released month |

Each routine writes `latest-results.json` to the repo root and emails `cam.conway@gmail.com` if anything is open. Monthly drop routines include urgency context — Wright's (14 tables) and Mainlander (25 Supper Club seats) sell out within minutes of release.

## Files

- `stl-friday-reservation-check/SKILL.md` — weekly scan logic (Wed–Sun)
- `stl-monthly-reservation-drop/SKILL.md` — monthly drop logic (parameterized by venue in routine prompt)
- `latest-results.json` — most recent scan output (rewritten on each run)

## Note on auto-booking

The routines do **not** book reservations. Resy and OpenTable forbid automation, and a flagged account would lose card-on-file privileges. The system surfaces openings + booking deep-links (e.g. `?date=YYYY-MM-DD&seats=2`) — Cam books in one tap from the email or dashboard.
