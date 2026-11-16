# Speedo (PMA Lahore Feeder Routes) — official poster data
Source: PMA posters (pma.masstransit Instagram) shared by Muneeb 2026-11-16. Stop order as printed, outbound direction. Coordinates found 2026-11-16 (Google Maps, incl. PMA-tagged "(RTxx)" stop markers) -> `speedo_stops_all.csv` (both directions: OUT = poster order, IN = return trip; confidence A/B/C). Road paths -> `road_speedo.json` (OSRM car routing through the stops, simplified ~4 m). Road notes + stops to confirm -> `road_paths_and_unsure.txt`. Not in index.html yet: routes get added one by one after the stops are checked.

| Route | From → To | Hours | Stops (poster) | Notes |
|---|---|---|---|---|
| RT-03 | Railway Station → Rana Town | 05:50–22:30 | 33 | Loops Lari Adda/Azadi/Texali/Bhatti (pairs "-2" are the other side), Shahdara Metro Station = MB link |
| RT-04 | R.A Bazar → Chungi Amar Sidhu | 05:25–23:30 | 14 | Walton/Packages/Qainchi; shares Qainchi, Ghazi Chowk with RT-07 |
| RT-05 | Bhatti Chowk → Shadbagh Underpass | 06:00–22:30 | 12 | Reverse direction has 13 stops (adds Azadi Chowk) |
| RT-07 | Depot Chowk (Green Town) → Chungi Amar Sidhu | 06:05–23:25 | 17 | First 9 stops = EV-03 corridor (Pulley, Minhaj, Hamdard, Nursery, Rehmat Eye, Pindi, Peco) — EV-03 coordinates can be reused |
| RT-08 | Canal → Daewoo Terminal | 06:00–22:06 | 20 | Poster draws 21 names (ends Jinnah Bus Terminal after Daewoo Terminal) — kept all 21. Shares canal stops with Route 26 |
| RT-09 | Railway Station → Gulshan Ravi OL Station | 05:30–22:40 | 15 | Chauburji & Gulshan Ravi = Orange Line links |
| RT-10 | Wahdat Road Station → Qartaba Chowk | not printed | 22 | Ichhra, Shama are street-level Ferozepur Rd stops (not the MB stations) |
| RT-11 | Main Market Gulberg → Babu Sabu | not printed | 28 | Urdu poster, 2 pages (names translated). Shares Bhekewal Morr with RT-10, Barkat Market/Campus Pul with RT-8; Niazi Adda near RT-3 Niazi Chowk |
| RT-12 | (saved earlier) | | | see RT12.csv |
| RT-19 | Paragon City → Railway Station | 05:30–22:00 | 32 | Via Cantt (Shami Rd, Fortress, CMH, Dharampura, Ghari Shahu) |

Still missing posters: routes 13–18, 20–28 Speedo (not the e-bus 20/26).
