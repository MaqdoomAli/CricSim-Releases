# CricSim Alpha 1.3 — IPL 25-season stress-test report

25 independently seeded **actual IPL 2026 tournaments** on the user's Alpha 1.2 game as updated with P12E tournament support. All 10 IPL teams and their built-in IPL 2026 squads were used. The tournament was played through production `simNextRound`, including all 4 playoffs every season. Player attributes and original rosters were not edited.

## Summary

| Measure | Result |
|---|---:|
| Seasons | **25** |
| Matches | **1,850** (1,750 league; 100 playoff) |
| Browser simulation errors | **0** |
| Mean runs per innings | **174.94** |
| Orange Cap mean / range | **657** / 550–722 |
| Purple Cap mean / range | **27.2** / 22–34 |
| Spontaneous retired-hurt incidents | **20** |
| Spontaneous bowler injuries | **8** |
| Injured batter returns | **15** (return events, not new injuries) |

Physical injury events: **28 in 1,850 matches ≈ 1.51%** of matches. Frequency is low as requested. The maximum one spontaneous injury per match was enforced by the P12E controller.

## Low-rated breakout batters

| Player | In-game batting rating | Best season | Reproducibility |
|---|---:|---:|---|
| Mitch Owen (PBKS) | 12/20 | **650 runs in 14 matches** (season 12) | One-off: only 1 season above 500 |
| Priyansh Arya (PBKS) | 12/20 | **600 runs** (season 6) | **8/25 seasons >=500**; average 437.1 runs across 25 seasons |
| Ayush Mhatre (CSK) | 12/20 | **589 runs** (season 11) | **5/25 seasons >=500**; average 390.0 |
| Abhishek Porel (DC) | 12/20 | **521 runs** (season 24) | 1/25 seasons >=500 |
| Matthew Short (CSK) | 12/20 | **502 runs** | 1/25 >=500 |
| Shahbaz Ahmed (LSG) | 10/20 | **235 runs** | No batter rated <=10 reached 250 |
| Naman Dhir (MI) | 10/20 | **228 runs** | Rare moderate contribution |

The original game has batting ratings 1–20. Rating 12 is a capable but not elite player, not a tailender. Frequent ~500-run seasons for Arya and Mhatre may reflect stable high-order opportunity plus archetype benefits, and deserves targeted calibration review before increasing/decreasing specific real-player ratings. At <=10 batting rating, the hierarchy remained strong.

## Low-rated breakout bowlers

| Player | In-game bowling rating | Best season | Context |
|---|---:|---:|---|
| Jacob Bethell (RCB) | 11/20 | **19 wickets** (season 2) | 239 legal balls in 14 matches; 17 in season 20 |
| Suyash Sharma (RCB) | 12/20 | **19 wickets** (seasons 9 & 17) | 215 and 318 legal balls; 4/25 seasons >=15 |
| Hardik Pandya (MI) | 12/20 | **18 wickets** (season 11) | 245 legal balls in 16 matches |
| Nitish Kumar Reddy (SRH) | 12/20 | **17 wickets** (season 11) | 198 legal balls |
| **Nitish Rana (DC)** | **9/20** | **16 wickets** (season 21) | **180 legal balls in 10 matches**; very high bowler usage for rating 9 |
| **Venkatesh Iyer (RCB)** | **10/20** | **15 wickets** (season 25) | Particularly noteworthy low-rated result |

**Audit candidate:** Nitish Rana bowled 30 overs in only 10 games as a rating-9 part-timer during season 21. Check captain bowling priority, bowler role declarations and availability of specialists before concluding whether this is a realistic lucky season or a planning defect. A single 16-wicket season itself does not prove a bug.

## Qualitative conclusions

- **Passed:** All 25 IPL league+playoff runs completed; scoring, stats and medical incident records were generated without throwing in headless browser test.
- **Passed:** The new P12E plumbing supports real main-game matches in T20/ODI/T10/The Hundred/Test/FC and 11-player protective behavior.
- **Watch:** 12/20 batters Priyansh Arya and Ayush Mhatre produce large seasons fairly often. Investigate batting-order and archetype amplifiers before changing user ratings.
- **Watch:** Occasional rating-9/10 part-time bowlers are given substantial overs and achieve high wicket totals; inspect captain usage.
- **Watch:** Orange Cap mean 657 and peak 722 should be compared with chosen *real IPL season benchmarks* before asserting the top-end is calibrated.
- **Not verified:** Live PlanetCricket BBCode installation, actual saved-universe browser migration, 6-player specialty match engines or a systematic every-format multi-season calibration.

Source of counts: `CricSim_IPL_25_Seasons_Raw_Results.json`, `CricSim_IPL_25_Season_Analysis.json`. All names and statistics above are generated game outcomes, **not actual IPL results**.
