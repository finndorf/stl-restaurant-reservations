---
name: stl-monthly-reservation-drop
description: Monthly alert when Wright's Tavern (12:01 AM) or The Mainlander (noon) drop new reservations on the 1st
---

It's the 1st of the month. A specific St. Louis restaurant just released its next month of Resy reservations. The routine prompt names which venue to check (Wright's or Mainlander). These sell out in minutes — act fast.

**Wright's Tavern** (Resy) — drops 12:01 AM on the 1st, 14 tables total
URL: https://resy.com/cities/clayton-mo/venues/wrights-tavern

**The Mainlander** (Resy) — drops noon on the 1st, 25 Supper Club seats
URL: https://resy.com/cities/st-louis-mo/venues/mainlander

**Steps:**
1. Identify the target month (the month that was just released). It is the 1st of THIS month, so the released month is the NEXT calendar month.
2. List every Wednesday, Thursday, Friday, Saturday, and Sunday in that target month.
3. For each date, check the venue page with `?date=YYYY-MM-DD&seats=2` and `&seats=4` query strings. Use WebFetch first; if the page returns shell HTML, fall back to WebSearch.
4. Time window: **6:00 PM – 7:30 PM** for Wright's Tavern. For The Mainlander, check the **6:00 PM fixed seating only** (it offers fixed seatings at 6:00 PM and 8:30 PM; 8:30 PM is out of scope). Note: The Mainlander may be closed Sundays — if no availability exists for Sunday, leave that date's availability empty.

**Output — write `latest-results.json` at the repo root:**
```json
{
  "checked_at": "2026-06-01T05:01:00Z",
  "scan_type": "monthly_drop",
  "venue": "Wright's Tavern",
  "target_month": "2026-07",
  "dates": ["2026-07-01", "2026-07-02", "2026-07-03", "2026-07-04", "2026-07-05", "2026-07-08", "2026-07-09", "2026-07-10", "2026-07-11", "2026-07-12", "2026-07-15", "2026-07-16", "2026-07-17", "2026-07-18", "2026-07-19", "2026-07-22", "2026-07-23", "2026-07-24", "2026-07-25", "2026-07-26", "2026-07-29", "2026-07-30", "2026-07-31"],
  "restaurants": [
    {
      "name": "Wright's Tavern",
      "platform": "Resy",
      "availability": [
        {"date": "2026-07-10", "party": 2, "times": ["6:45 PM"], "booking_url": "https://resy.com/cities/clayton-mo/venues/wrights-tavern?date=2026-07-10&seats=2"}
      ],
      "notes": ""
    }
  ]
}
```

**Commit and push:**
```
git add latest-results.json
git -c user.email="agent@stl-reservations" -c user.name="STL Reservations Agent" commit -m "monthly drop $(date -u +%Y-%m-%d) — <venue>"
git push origin main
```

**Email Cam IMMEDIATELY if any availability is found:**
Use the Gmail connector. Send to cam.conway@gmail.com with subject `"🚨 <Venue> — book now"` and a plain-text body listing each open Friday with its `booking_url`. Urgency is the whole point — these sell out in minutes.

If nothing is open: still write the JSON file with empty `availability`, but do NOT email.
