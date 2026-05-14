---
name: stl-friday-reservation-check
description: Weekly check for Wed–Sun evening openings at 6 St. Louis restaurants
---

Check for dinner availability at 6 St. Louis restaurants. Look for a table for 2 or 4 people on any of the next 2 weeks of Wednesday through Sunday evenings between 6:30 PM and 7:30 PM.

**Restaurants:**
1. Wright's Tavern (Resy): https://resy.com/cities/clayton-mo/venues/wrights-tavern
2. Louie on DeMun (Resy): https://resy.com/cities/clyt/louie
3. The Mainlander (Resy): https://resy.com/cities/st-louis-mo/venues/mainlander
4. Robin Restaurant (Resy): https://resy.com/cities/st-louis-mo/venues/robin-restaurant
5. The Crossing (OpenTable): https://www.opentable.com/the-crossing-clayton
6. Acero (OpenTable): https://www.opentable.com/acero-saint-louis

**How to check:**
For each restaurant URL, use WebFetch and WebSearch to gather availability. Resy and OpenTable expose deep-link URLs that can be parameterized — for Resy, append `?date=YYYY-MM-DD&seats=2` (or `seats=4`). When WebFetch returns shell HTML, fall back to WebSearch with the venue name plus the target date and "available" to surface third-party indicators.

**Criteria:**
- The next 14 calendar days, filtered to Wednesday through Sunday only (roughly 10 nights)
- Party of 2 first, then party of 4
- Time window: 6:30 PM – 7:30 PM only

**Output — write `latest-results.json` at the repo root with this exact shape:**
```json
{
  "checked_at": "2026-05-19T13:00:00Z",
  "scan_type": "weekly",
  "dates": ["2026-05-20", "2026-05-21", "2026-05-22", "2026-05-23", "2026-05-24", "2026-05-27", "2026-05-28", "2026-05-29", "2026-05-30", "2026-05-31"],
  "restaurants": [
    {
      "name": "Wright's Tavern",
      "platform": "Resy",
      "availability": [
        {"date": "2026-05-22", "party": 2, "times": ["6:45 PM", "7:15 PM"], "booking_url": "https://resy.com/cities/clayton-mo/venues/wrights-tavern?date=2026-05-22&seats=2"}
      ],
      "notes": ""
    }
  ]
}
```
If a restaurant has no availability, set `availability: []` and put any uncertainty in `notes` (e.g., "JS page did not render — check manually").

**Commit and push:**
After writing the file, run:
```
git add latest-results.json
git -c user.email="agent@stl-reservations" -c user.name="STL Reservations Agent" commit -m "weekly scan $(date -u +%Y-%m-%d)"
git push origin main
```

**Email Cam if anything is open:**
If any `availability[]` is non-empty, use the Gmail connector to send a message to cam.conway@gmail.com with subject `"STL Dinner — openings found"` and a body listing each opening with its booking URL. If everything is empty, do NOT email.
