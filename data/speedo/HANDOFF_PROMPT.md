# Lahore Transit app — bus stop lookup task (handoff prompt)

Copy everything in this file into a new Claude chat. It contains the full context and instructions.

---

## Who I am and what this is for
I'm Muneeb, from Lahore. I'm building **Lahore Transit**, a Jakdojade-style public-transport journey planner for Lahore (live at https://muneeb178.github.io/effective-potato-x43/). It already has the Metrobus, Orange Line, PTC electric bus EV-03, and PMA Electric Routes 20 and 26. Next I'm adding the **Speedo feeder bus routes** (Punjab Masstransit Authority, "Lahore Feeder Routes System").

For each route I have the **official stop list in order** (from PMA posters). What's missing are the **coordinates of each stop, in both directions**. Your job is to find them, as accurately as possible, and give them back in the exact format below.

**This is a data-collection job only.** Please don't write code, build or change the app, or suggest app changes. No GitHub or app access is needed. Just research and return the data; another assistant will put it into the app.

## Rules (important — please follow exactly)
1. **Official order and names win.** Keep the stop order and names from the lists below. Don't add, drop or reorder stops. If you think a name is wrong or a stop doesn't exist, keep the row and say so in `notes`.
2. **Speedo buses run on ordinary roads and stop at roadside stops.** They never use the Metrobus or Orange Line tracks or stations. When a stop is named after a Metrobus or Orange Line station (e.g. "Shama", "Ichhra", "Canal", "Chauburji OL Station", "Shahdara Metro Station", "Wahdat Road Station"), give the **roadside bus stop on the street next to/under the station**, not the station platform.
3. **Put the point on the road the bus actually drives on**, on the correct side for the stated direction of travel (Pakistan drives on the LEFT, so the stop for a bus heading north is on the west side of the road). Not inside a building, park or neighbourhood street.
4. **Use the best source available, in this order:**
   - (A) A Google Maps **bus stop / transit stop** marker with that name, or a PMA/Speedo official source → confidence `A`
   - (B) The named landmark/junction itself (chowk, college, hospital, market) placed on the route road at that landmark → confidence `B`
   - (C) Your estimate between neighbouring stops along the road → confidence `C` (say why in `notes`)
5. **Coordinates:** decimal degrees, 5–6 decimals, `lat,lng` (Lahore is about lat 31.3–31.7, lng 74.1–74.6). Sanity-check: consecutive stops are usually 300–1500 m apart and in order along the road. If two stops come out more than ~3 km apart or out of order, re-check.
6. **Both directions, separately.** Every route runs both ways on ordinary roads, so each stop usually has TWO points, one on each side of the road (on the left-hand kerb for that direction of travel). They are often **not opposite each other**: the return stop can be 50–300 m away, wherever the road allows (e.g. a "Club Road" stop on one side may sit at a different spot from the other side's "Club Road" stop). Sometimes a stop exists **only in one direction**, or has a **different name** the other way, or the return trip uses a **different road** (one-way streets, U-turn points). Collect:
   - `OUT` rows: the direction printed on the poster (first stop → last stop), in that order.
   - `RET` rows: the return trip, in the order the bus meets them coming back. Leave out stops that don't exist on the way back, and add ones that only exist on the way back. Say so in `notes` ("only on return", "named X on return").
   - On divided roads and Canal Road, the two directions are on different carriageways/banks, so the points must differ.
7. **Reuse known coordinates.** The KNOWN STOPS section lists stops already verified in the app. If a Speedo stop is the same place (e.g. Route 7 shares stops 1–10 with EV-03; Route 8 shares Canal Road stops with Route 26), copy those coordinates and mark the source as `known:<line>`.
8. **Also record the road path, for each direction.** For each route, list in order the roads the bus drives along and any notable turns or U-turns (e.g. "Ferozepur Rd N → right onto Canal Bank Rd (north bank) → U-turn at Thokar"). This is needed to draw the route on real roads.
9. Don't invent precision. A `C` estimate is fine; a fake `A` is not.

## How to look things up
- If you have a browser or computer-use tool: open Google Maps and search "<stop name> bus stop Lahore" or "<landmark>, <area>, Lahore". The coordinates are in the URL after `@` or `!3d<lat>!4d<lng>`, or right-click → copy coordinates.
- With web search/fetch only: you can use OpenStreetMap's search, e.g. `https://nominatim.openstreetmap.org/search?q=Kalma+Chowk+Lahore&format=json&limit=3` (returns lat/lon), and web search for the landmark plus "Lahore". Then place the point on the route road at that landmark.
- If you can't find something, **ask me** (Muneeb). I can drop a Google Maps pin and send the coordinates. Batch your questions: send one list of the stops you couldn't place, not one message per stop.
- Work **one route at a time**. After each route, give me the finished CSV block before starting the next, so nothing is lost if the chat runs out.

## Output format (exactly this, one CSV block per route)
```
route,dir,seq,stop_en,lat,lng,confidence,source,notes
RT-07,OUT,1,Depot Chowk (Green Town),31.430920,74.311294,A,known:EV3,
RT-07,OUT,2,Pulley Bus Stop,31.436854,74.314476,A,known:EV3 (Pully),
...
RT-07,RET,1,Chungi Amar Sidhu,…,…,B,Google: landmark,return stop is ~120 m south of the OUT stop
...
```
After each route's CSV, add:
```
ROAD PATH RT-07 OUT: <road 1> → <road 2> → … (turns/U-turns)
ROAD PATH RT-07 RET: … (only if different from OUT reversed)
UNSURE RT-07: <list of stops you'd like me to confirm>
```

## Routes to do (priority order: 7, 8, 4, 9, 10, 12, 19, 3, 5, 11)

### RT-07: Depot Chowk (Green Town) → Chungi Amar Sidhu  (hours 06:05–23:25, 17 stops)
Stops 1–10 are the SAME road as PTC EV-03 (Green Town → Pulley → Minhaj → Hamdard → Somi → Nursery → Rehmat Eye → Pindi Stop → Treet → Peco Mor). Exact coordinates for most of these are in the KNOWN STOPS section below — reuse them. Then Kot Lakhpat Railway Station → Phatki / Phattak Mandi → Model Town Katchery → Qainchi → Ghazi Chowk → Chungi Amar Sidhu.

| # | Stop |
|---|---|
| 1 | Depot Chowk (Green Town) |
| 2 | Pulley Bus Stop |
| 3 | Minhaj University |
| 4 | Hamdard Chowk |
| 5 | Somi Center Bus Stop |
| 6 | Nursery Bus Stop |
| 7 | Rehmat Eye Hospital |
| 8 | Pindi Stop |
| 9 | Treet Stop |
| 10 | Peco Mor |
| 11 | Kot Lakhpat Railway Station |
| 12 | Phatki Stop |
| 13 | Phattak Mandi Stop |
| 14 | Model Town Katchery |
| 15 | Qainchi Stop |
| 16 | Ghazi Chowk |
| 17 | Chungi Amar Sidhu |

### RT-08: Canal → Daewoo Terminal  (hours 06:00–22:06, 21 stops)
Starts at Canal (the street-level stop near the Canal Metrobus station on Ferozepur Road) → Qaddafi Stadium → Kalma Chowk → Barkat Market → Campus Pull → then along Canal Road (Gate 4, Hailey College, IBA, Shah Di Khoi, Jinnah Hospital, Wafaqi Colony, Doctor Hospital, PCSIR) → Johar Town G-4 → Hanjarwal → Mughal Eye Hospital → Canal View → Judicial Colony → Thokar Niaz Baig → Daewoo Terminal → Jinnah Bus Terminal. Poster says 20 stops but draws 21 names; keep all 21. Many Canal Road stops are shared with Electric Route 26 (coordinates in KNOWN STOPS).

| # | Stop |
|---|---|
| 1 | Canal |
| 2 | Qaddafi Stadium |
| 3 | Kalma Chowk |
| 4 | Barkat Market |
| 5 | Campus Pull |
| 6 | Gate No. 4 |
| 7 | Hailey College |
| 8 | IBA Stop |
| 9 | Shah Di Khoi |
| 10 | Jinnah Hospital |
| 11 | Wafaqi Colony |
| 12 | Doctor Hospital |
| 13 | PCSIR-1 |
| 14 | G-4 Johar Town |
| 15 | Hanjarwal |
| 16 | Mughal Eye Hospital |
| 17 | Canal View Gate 1 |
| 18 | Judicial Colony |
| 19 | Thokar Niaz Baig |
| 20 | Daewoo Terminal |
| 21 | Jinnah Bus Terminal |

### RT-04: R.A Bazar → Chungi Amar Sidhu  (hours 05:25–23:30, 14 stops)
Cantt (R.A Bazar) → Walton Road → Bab-e-Pakistan → Packages → Qainchi → Ghazi Chowk → Chungi Amar Sidhu (Ferozepur Road).

| # | Stop |
|---|---|
| 1 | R.A Bazar |
| 2 | Nadeem Chowk |
| 3 | T-Stop |
| 4 | Defence Morr |
| 5 | Korray Stop |
| 6 | Shareef Market |
| 7 | College Stop |
| 8 | Walton |
| 9 | Bab-e-Pakistan |
| 10 | Workshop |
| 11 | Packages |
| 12 | Qainchi |
| 13 | Ghazi Chowk |
| 14 | Chungi Amar Sidhu |

### RT-09: Railway Station → Gulshan Ravi Orange Line station  (hours 05:30–22:40, 15 stops)
Railway Station → Haji Camp → Police Line → Shimla Pahari → LDA Plaza → Aiwan-e-Iqbal → Zoo → Charing Cross (Mall Road) → Ganga Ram → Waris Road → Qartaba Chowk → Chauburji (Orange Line) → Poonch House → Gulshan Ravi (Orange Line). Bus stops are on the road BELOW the Orange Line stations, not the stations.

| # | Stop |
|---|---|
| 1 | Railway Station |
| 2 | Haji Camp |
| 3 | Police Line |
| 4 | Shimla Pahari |
| 5 | Shimla Pahari-2 |
| 6 | LDA Plaza |
| 7 | Aiwan-e-Iqbal |
| 8 | Lahore Zoo |
| 9 | Charing Cross |
| 10 | Ganga Ram Hospital |
| 11 | Waris Road |
| 12 | Qartaba Chowk |
| 13 | Chauburji OL Station |
| 14 | Poonch House |
| 15 | Gulshan Ravi OL Station |

### RT-10: Wahdat Road Station → Qartaba Chowk  (hours not printed, 22 stops)
Wahdat Road (Orange Line station area) → Multan Chungi → Mustafa Town → Karim Block → Sikandar Block → Raza Block (Allama Iqbal Town) → PU Examination Hall → Science College → Bhekewal Morr → Wahdat Colony → … → Karma Abad → Aab Para → Canal → Rehman Pura → Ichhra → Shama → LOS → Qartaba Chowk. Ichhra and Shama are the STREET-LEVEL stops on Ferozepur Road, NOT the Metrobus stations above.

| # | Stop |
|---|---|
| 1 | Wahdat Road Station |
| 2 | Multan Chungi |
| 3 | Elephant Link Road |
| 4 | Mustafa Town |
| 5 | Karim Block Market |
| 6 | Sikander Block |
| 7 | Raza Block |
| 8 | PU Examination Hall |
| 9 | Science College |
| 10 | Bhekewal Morr |
| 11 | Wahdat Colony |
| 12 | School Stop |
| 13 | Bank Stop |
| 14 | Naqsha Stop |
| 15 | Karma Abad |
| 16 | Aab Para Market |
| 17 | Canal |
| 18 | Rehman Pura |
| 19 | Ichhra |
| 20 | Shama |
| 21 | LOS |
| 22 | Qartaba Chowk |

### RT-12: R.A Bazar → Nasir Bagh  (hours see poster, 23 stops)
Cantt (R.A Bazar, PAF Market, Girja Chowk, Alla-ud-Din Rd, Fortress Stadium, Mian Mir) → Gymkhana → Canal Bridge → Aitchison College → Club Road → PC Hotel → Governor House → Zoo → Charing Cross → Regal → GPO → Anarkali → PU Old Campus → Govt College → Nasir Bagh. GPO/Anarkali here are the street stops on Mall Road, not the underground Orange Line stations.

| # | Stop |
|---|---|
| 1 | R.A Bazar |
| 2 | PAF Market |
| 3 | Telephone Exchange |
| 4 | Girja Chowk |
| 5 | Alla-ud-Din Road |
| 6 | Afshan Chowk |
| 7 | Rahat Bakery |
| 8 | Fortress Stadium |
| 9 | Mian Mir |
| 10 | Gym Khana |
| 11 | Canal Bridge |
| 12 | Aitchison College |
| 13 | Club Road |
| 14 | PC Hotel |
| 15 | Governor House |
| 16 | Lahore Zoo |
| 17 | Charing Cross |
| 18 | Regal Chowk |
| 19 | GPO |
| 20 | Anarkali |
| 21 | PU Old Campus |
| 22 | Government College |
| 23 | Nasir Bagh |

### RT-19: Paragon City → Railway Station  (hours 05:30–22:00, 32 stops)
Barki Road side (Paragon City, Dograi, Baoo Wala) → Cantt (Askari X, Rangers HQ, Jawad Cheema Rd, Army Museum, Ayub Stadium, APS, PAF Market, R.A Bazar, Nadeem Chowk, Sarwar Road, Shami Road, Munir Road, Abid Majeed Road, Fortress Stadium, Hyperstar, CMH, Infantry Road / Mian Mir) → Dharampura Bridge → Ghari Shahu → Railway HQ → Railway Station.

| # | Stop |
|---|---|
| 1 | Paragon City |
| 2 | Dograi |
| 3 | Baoo Wala |
| 4 | Mehfooz Shaheed Garrison |
| 5 | Quaid Check Post |
| 6 | Askari-X |
| 7 | Ranger Headquarter |
| 8 | Jawad Cheema Road |
| 9 | Army Museum |
| 10 | Ayub Stadium |
| 11 | APS Sarfaraz Rafique Road |
| 12 | PAF Market |
| 13 | R.A Bazar |
| 14 | Nadeem Chowk |
| 15 | Serwar Road |
| 16 | D-Mess Shami Road |
| 17 | F-Mess Shami Road |
| 18 | Munir Road |
| 19 | Alla-ud-Din Road |
| 20 | Abid Majid Road |
| 21 | Fortress Stadium |
| 22 | Navy Selection Center-Hyperstar |
| 23 | CMH |
| 24 | Infantry Road Mian Mir |
| 25 | Dharampura Bridge |
| 26 | Euro Store |
| 27 | Police Station Ghari Shahu |
| 28 | Ghari Shahu Bazar |
| 29 | Ghari Shahu Chowk |
| 30 | Railway Headquarters |
| 31 | Bohar Wala Chowk |
| 32 | Railway Station |

### RT-03: Railway Station → Rana Town  (hours 05:50–22:30, 33 stops)
Loops around Lari Adda / Azadi Chowk / Texali / Bhatti Chowk (stops marked "-2" are the same junction on the other side of the road, used in the other direction of the loop). Then Niazi Chowk → Shahdara (Metrobus terminal area) → Kot Shahab-ud-Din → Wandala Road → Rana Town (north of the Ravi, Sheikhupura Road side).

| # | Stop |
|---|---|
| 1 | Railway Station |
| 2 | Railway Station-2 |
| 3 | Nawaz Sharif |
| 4 | Sheranwala Gate |
| 5 | Kashmiri Gate |
| 6 | Lari Adda |
| 7 | Lari Adda Gol Chakar |
| 8 | Azadi Chowk |
| 9 | Texali Chowk |
| 10 | Bhatti Chowk |
| 11 | Bhatti Chowk-2 |
| 12 | Texali Chowk-2 |
| 13 | Azadi Chowk 2 |
| 14 | Timber Market |
| 15 | Metro |
| 16 | Niazi Chowk |
| 17 | Niazi Interchange |
| 18 | Shahdara Metro Station |
| 19 | Shahdara Lari Adda |
| 20 | Kot Shahab-ud-Din |
| 21 | Shahdara Railway Station |
| 22 | Wandala Road |
| 23 | Kashmir Park |
| 24 | Machas Factory |
| 25 | Barkat Town |
| 26 | Kala Khatai Road |
| 27 | Imamia Colony |
| 28 | Feroz Wala |
| 29 | Rachna Town |
| 30 | College Road |
| 31 | Haider Road |
| 32 | Nazim Road |
| 33 | Rana Town |

### RT-05: Bhatti Chowk → Shadbagh Underpass  (hours 06:00–22:30, 12 stops)
Walled-city edge: Bhatti → Texali → Lari Adda → Badami Bagh → Sabzi Mandi (Ravi Road side) → Shadbagh. Reverse direction has 13 stops (adds Azadi Chowk).

| # | Stop |
|---|---|
| 1 | Bhatti Chowk |
| 2 | Texali Chowk |
| 3 | Lari Adda Gol Chakar |
| 4 | Badami Bagh |
| 5 | Sabzi Mandi |
| 6 | Usman Chowk |
| 7 | Lohay Wali Pulley |
| 8 | Ali Hajvery Shadi Hall |
| 9 | Masoom Shah Darbar |
| 10 | Madina Chowk |
| 11 | Rajput Park |
| 12 | Shadbagh Underpass |

### RT-11: Main Market Gulberg → Babu Sabu  (hours not printed, 28 stops)
Gulberg (Main Market, MM Alam, Liberty, Hafeez Centre) → Barkat Market → Campus Pul → PU New Campus → Shaikh Zayed → Bhekewal Morr → Pilot School → Farooq Hospital → Gulshan-e-Iqbal → Dubai Chowk → Flat Stop → Kali Kothi → Scheme Morr → Grand Battery → Chowk Yateem Khana → City Bus Stand (Bund Road) → Shezan Factory → Niazi Adda → Bilal Daewoo → Babu Sabu (Bund Road / M-2 interchange). Names were translated from Urdu; real local spellings may differ.

| # | Stop |
|---|---|
| 1 | Main Market Gulberg |
| 2 | McDonald's-1 |
| 3 | Siddique Trade Center |
| 4 | McDonald's-2 |
| 5 | Main Market-2 |
| 6 | Mini Market |
| 7 | Hafeez Centre |
| 8 | Liberty Chowk |
| 9 | Centre Point |
| 10 | Barkat Market |
| 11 | Campus Pul |
| 12 | Punjab University New Campus |
| 13 | Shaikh Zayed Hospital |
| 14 | Bhekewal Morr |
| 15 | Pilot School |
| 16 | Farooq Hospital |
| 17 | Gulshan-e-Iqbal |
| 18 | Dubai Chowk |
| 19 | Flat Stop |
| 20 | Kali Kothi Stop |
| 21 | Scheme Morr |
| 22 | Grand Battery Stop |
| 23 | Chowk Yateem Khana |
| 24 | City Bus Stand |
| 25 | Shezan Factory |
| 26 | Niazi Adda |
| 27 | Bilal Daewoo |
| 28 | Babu Sabu |

## KNOWN STOPS (already in the app — reuse when a Speedo stop is the same place. These are mostly ONE direction only, so the other side of the road still needs its own point)
Format: name, lat, lng

**PTC EV-03 (Depot Green Town → Railway Station; exact = GPS/Google-verified)**
```
Depot Bus Stop (Green Town), 31.432172, 74.312041
Pully Stop, 31.436854, 74.314476
Minhaj University, 31.443570, 74.318242
Hamdard Chowk, 31.447238, 74.320235
Nursery Stop, 31.452740, 74.323334
Riphah University, 31.454754, 74.324479
Pindi Stop, 31.459989, 74.326115
Government Employees Society, 31.461729, 74.321797
Honda Morr/Link Road, 31.464770, 74.314253
Township Bazar/Mochipura, 31.465648, 74.311678
Akbar Chowk, 31.467562, 74.306816
Shouk Chowk, 31.470278, 74.303244
Ravi Chowk, 31.474570, 74.302210
Johar Shadi Hall, 31.479650, 74.299210
Jinnah Hospital, 31.486037, 74.294288
Shah Di Khui, 31.490077, 74.297210
IBA, 31.493223, 74.301104
Hailey College, 31.496720, 74.304663
PU Gate 4, 31.501010, 74.308656
Campus Pull, 31.504390, 74.311010
Punjab College/New Muslim Town, 31.513330, 74.319890
Ichhra Pull/Muslim Town Morr, 31.520533, 74.323319
Rehmanpura, 31.524070, 74.323620
Fazlia Colony, 31.528510, 74.322230
Ichra Station, 31.532573, 74.321229
Shama Station, 31.539101, 74.319085
Shadman Itwar Bazar, 31.538480, 74.324200
Shadman Market, 31.539500, 74.328850
Shadman Chowk/LCWU, 31.542720, 74.327810
Eden Center, 31.543560, 74.321880
Mozang Chungi, 31.546500, 74.317560
Qartaba Chowk/Abid Market, 31.548156, 74.315106
Waris Road, 31.551540, 74.318770
Gangaram Hospital, 31.554302, 74.320208
Charing Cross, 31.559918, 74.323705
Lahore Zoo, 31.558640, 74.326746
Bagh e Jinnah, 31.557700, 74.328900
Governor House, 31.554603, 74.334590
Club Chowk, 31.551212, 74.341155
Muslim League House, 31.555616, 74.340593
Roznama Jang, 31.560370, 74.338052
Shimla Hill, 31.562111, 74.334321
Police Line, 31.567862, 74.335490
Haji Camp, 31.570389, 74.335236
Railway Station, 31.577400, 74.336480
```

**Electric Route 26 (Canal Road; some positions estimated, all snapped onto the road)**
```
Jinnah Bus Terminal, 31.467500, 74.241000
Thokar Niaz Baig, 31.471090, 74.240340
Canal View Gate 1, 31.473350, 74.250450
Minhaj-ul-Hussain, 31.474800, 74.256760
Fatimiyya Road, 31.477000, 74.266000
PCSIR-1, 31.479240, 74.275100
Doctor Hospital, 31.481550, 74.282040
Wafaqi Colony, 31.484670, 74.289030
CSAS Stop, 31.487190, 74.293290
Shah Di Khoi, 31.490080, 74.297210
IBA Stop, 31.493220, 74.301100
Hailey College, 31.496720, 74.304660
Gate No. 4, 31.501010, 74.308660
Campus Pull, 31.504030, 74.311520
New Garden Town, 31.512640, 74.319890
Canal Metro Bus Station, 31.520060, 74.326320
FC College, 31.526500, 74.332580
Shah Jamal, 31.532320, 74.338220
Kinnaird College, 31.537730, 74.344190
Jail Road, 31.542600, 74.348180
GOR, 31.546610, 74.351690
Mall Road, 31.549860, 74.354730
Dharampura, 31.553440, 74.358100
Chobucha Pull, 31.557810, 74.364760
Dakkana Phattak, 31.560870, 74.372880
Mughal Pura, 31.562510, 74.380710
Jafri Pull, 31.563660, 74.386060
Lal Pull, 31.564370, 74.390030
Nabi Pura, 31.565370, 74.394900
Nawan Pull, 31.566460, 74.399920
Fateh Garh Pull, 31.567530, 74.405670
Canal Bank Society, 31.568650, 74.411100
Taj Bagh, 31.570490, 74.420190
Harbanspura Morr, 31.572500, 74.428000
Harbanspura Bridge, 31.573570, 74.434000
Harbanspura Police Station, 31.575120, 74.442580
Press Club Society, 31.576500, 74.450000
Rani Pind, 31.578420, 74.458860
TulsPura, 31.580000, 74.466000
Rizwan Garden, 31.582300, 74.478240
Jallo Park, 31.583250, 74.482290
Sozo Water Park, 31.584050, 74.486820
Khaira Pull, 31.585220, 74.495940
```

**Electric Route 20 (Raiwind Road)**
```
Jinnah Bus Terminal, 31.467500, 74.241000
Thokar Niaz Baig, 31.466800, 74.243200
Thokar Niaz Baig Morr, 31.468800, 74.242500
Ali Town Station, 31.465070, 74.243870
Fatima Masjid, 31.459070, 74.244700
Katchi Kothi Stop, 31.451590, 74.245740
QazzalBash Chowk, 31.449090, 74.242240
Rahima Baad, 31.445560, 74.236390
Agrics Town, 31.441430, 74.231960
Ali-Raza Baad, 31.434100, 74.232190
P.U Employees Society, 31.431400, 74.231460
University of South Asia, 31.426500, 74.230450
LDA Avenue Chowk, 31.421350, 74.229780
UMT Stop, 31.418940, 74.229750
Sher-Shah Road, 31.410600, 74.229890
Mumtaz Bakhtawar Hospital, 31.405500, 74.229900
Bhubhtian Pind, 31.401240, 74.230010
Bhubhtian Chowk, 31.396600, 74.229830
Millat Factory, 31.389280, 74.230190
Bhatta Pind, 31.383520, 74.230160
Riphah University, 31.377260, 74.230370
Fizaia Housing Scheme, 31.368320, 74.232710
Adda Plot Gol Chakkar, 31.365210, 74.233500
Adda Plot Market, 31.363640, 74.234420
Janjatey Pind, 31.358500, 74.235700
Al Kabir Town, 31.353270, 74.237110
Araian Pind, 31.345730, 74.239110
Araian Adda, 31.338800, 74.236890
Superior University, 31.334270, 74.235280
Kot Araian, 31.327520, 74.231670
Bahria Orchard-1, 31.321900, 74.228650
Bahria Orchard-2, 31.315970, 74.225470
Tibba Bur Singh, 31.309090, 74.224570
Haier Factory Stop, 31.303110, 74.224420
Noor Pajian, 31.299880, 74.224460
Amin Pura, 31.288020, 74.224940
Graduate College, 31.276000, 74.225010
Khara Chowk, 31.263920, 74.224650
Sue Asal Phatak Raiwind, 31.251760, 74.221470
```

**Metrobus STATIONS (elevated – NOT bus stops; street stops are next to/under them)**
```
Shahdara, 31.614444, 74.291278
Niazi Chowk, 31.602036, 74.299425
Timber Market, 31.594603, 74.302022
Azadi Chowk, 31.590712, 74.306381
Bhatti Chowk, 31.580653, 74.306251
Katchery, 31.574525, 74.306410
Civil Secretariat, 31.568883, 74.304381
MAO College, 31.561715, 74.306661
Janazgah, 31.553807, 74.311600
Qartaba Chowk, 31.548731, 74.315093
Shama, 31.538896, 74.319186
Ichhra, 31.531502, 74.321419
Canal, 31.519256, 74.327070
Qaddafi Stadium, 31.512022, 74.328812
Kalma Chowk, 31.504775, 74.331727
Model Town, 31.497500, 74.334397
Naseerabad, 31.487064, 74.338735
Ittefaq Hospital, 31.479278, 74.342000
Qainchi, 31.464367, 74.348214
Ghazi Chowk, 31.457051, 74.351205
Chungi Amar Sidhu, 31.450361, 74.353336
Kamahan, 31.440913, 74.355274
Atari Saroba, 31.431058, 74.356728
Nishtar Colony, 31.419825, 74.358384
Youhanabad, 31.412916, 74.359400
Dulu Khurd, 31.405577, 74.360481
Gajjumata, 31.398417, 74.361528
```

**Orange Line STATIONS (elevated/underground – NOT bus stops)**
```
Ali Town, 31.464321, 74.243826
Thokar Niaz Baig, 31.473642, 74.241825
Canal View, 31.480695, 74.249204
Hanjarwal, 31.486296, 74.258350
Saad Mughal Road, 31.494689, 74.263366
Awan Town, 31.503310, 74.268284
Sabzazar, 31.510123, 74.272844
Khatam-e-Nabuwat, 31.517424, 74.277647
Salahuddin Road, 31.524599, 74.283314
Bund Road, 31.532240, 74.287648
Samanabad, 31.539754, 74.292818
Gulshan-e-Ravi, 31.544898, 74.295299
Chauburji, 31.552328, 74.302733
Anarkali, 31.562483, 74.309336
GPO, 31.565554, 74.314244
Lakshmi, 31.568612, 74.327213
Lahore Junction, 31.572298, 74.336153
Sultanpura, 31.575772, 74.350794
UET, 31.577274, 74.360802
Baghbanpura, 31.579416, 74.370783
Shalamar Garden, 31.583474, 74.379751
Pakistan Mint, 31.585283, 74.389258
Mahmood Booti, 31.586829, 74.402435
Salamatpura, 31.587909, 74.413037
Islam Park, 31.588879, 74.422834
Dera Gujran, 31.589738, 74.431301
```

## Notes on Lahore geography that help
- Metrobus runs Shahdara ↔ Gajjumata along Ferozepur Road (elevated in the north, street-level corridor in the south). Orange Line runs Ali Town ↔ Dera Gujran (elevated; Anarkali and GPO are underground).
- "Pull/Pul" = canal bridge on Canal Bank Road (each has a stop on the canal road at the bridge). "Chowk" = junction. "Morr/Mor" = turning. "Chungi" = old toll point.
- Chungi Amar Sidhu, Ghazi Chowk, Qainchi and Model Town Katchery are on or near Ferozepur Road in the south. Qartaba Chowk is at Jail Road / Lytton Road.

When you've finished all routes (or the chat is getting long), give me **one combined CSV** of everything done so far, plus all the ROAD PATH and UNSURE lines. I'll paste that back to my main chat.
