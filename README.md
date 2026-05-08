# STL Restaurant Reservations

Automated Friday night reservation checker for 6 St. Louis restaurants, powered by Claude (Cowork mode).

## Restaurants Tracked
| Restaurant | Platform | Reservation Release |
|---|---|---|
| Wrights Tavern | Resy | 1st of month, 12:01 AM |
| The Mainlander | Resy | 1st of month, noon |
| Robin Restaurant | Resy | Rolling 60-day window |
| Louie on DeMun | Resy | Standard rolling |
| The Crossing | OpenTable | Standard rolling |
| Acero | OpenTable | Standard rolling |

## Automated Tasks

### Weekly (Every Monday at 8 AM)
Scans all 6 restaurants for the next 4 upcoming Fridays. Looks for party of 2 or 4 between 6:30-7:30 PM.

### Monthly (1st of Each Month at 12:01 AM)
Hits Wrights Tavern and The Mainlander the moment their new month drops on Resy. These sell out in minutes.

## Files
- `stl-friday-reservation-check/SKILL.md` - Weekly Friday availability scan
- `stl-monthly-reservation-drop/SKILL.md` - Monthly drop alert for Wrights and Mainlander
