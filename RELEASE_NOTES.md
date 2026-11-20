# CricSim v12 Alpha 1.4 — First public PC playtest

**Platform:** Offline HTML game for desktop Chrome/Edge · **Status:** Alpha playtest.

## Core systems

- **Matches:** T20, ODI, Test, FC, T10, The Hundred and custom-over formats, simulated delivery by delivery.
- **Gameplay controls:** Playing XIs, batting order, bowling eligibility/phase preferences, separate batting/bowling ratings and player archetypes.
- **World Pool:** 10,877 player identities, 72,302 format/era performance profiles, 415 teams, 3,295 squad snapshots and 342 tournament catalogue entries (not every entry complete or one-click playable).
- **Competitions:** IPL-style playoffs, knockouts, groups, Swiss competitions and the multi-stage Competition Architect, plus domestic competition definitions.
- **Match conditions:** Pitch, weather, simulation era, rain, DRS, free hits, Super Overs and optional Impact Player where applicable.
- **Match presentation:** Full scorecards, fall of wickets, delivery commentary, Match Centre, match stories, stats, records, standings and qualification analysis.
- **PlanetCricket exporter:** Match reports, squads, fixtures, tables and statistics, with plain BBCode fallback. Enhanced style requires forum admin installation.
- **Injuries and retirements:** Opt-in, limited player-care logic for the normal 11-player match modes. Special six-player formats are not fully integrated.
- **New in Alpha 1.4:** Optional Portable Save Folder, verified rolling full-universe snapshots, restore-from-folder and JSON export/import fallback.

## Stress testing and cricket calibration

The **25-season / 1,850-match IPL run** was the **latest** stress test, not the project's only one. Earlier documented audits include:

- **V22:** 30 full 45-match ODI tournaments (1,350 matches) testing batting/bowling leaders and team-scoring tails, with unresolved excess fifties and all-outs.
- **Phase 2U:** Seven pitch surfaces tested with 2,240 simulated first innings; 77-match English One-Day Cup and 63-match Sri Lankan FC seasons; plus a 30-seed BPL season-leader audit.
- **v11.96:** 48 synthetic 70-match T20 leagues (3,360 fixtures) comparing season leader tails across 2006, 2015 and 2026.
- **v11.97:** 864 complete red-ball matches across both baseline and candidate, testing captaincy defaults, batting depth, durations and weather.
- **V16:** 100 full IPL seasons — **7,400 matches / 14,800 innings**, with calibration against IPL Cricsheet. The run exposed excess all-outs and too few 200+ scores.
- **V18:** 90 IPL seasons — **6,660 matches** measuring Orange/Purple Cap leaders, century rates and freak individual innings.
- **V19:** **1,200 seeded ODIs** to measure scoring phases, century rates and wicket hauls.
- **v11.93/v11.95:** 4,554 first-innings observations across 23 years/three formats, 138 complete-match smoke tests; later 978 paired innings per version and 69 full matches.
- **P10.7:** 408 source-backed 2026 Hundred real-XI matches per engine as a held-out candidate comparison.
- **v11.66:** 998 genuine ball-by-ball matches across T20, T10, Hundred, ODI, Test and FC (54 tournament editions, 313,975 legal balls), with no reported mismatches in the audited score/stat invariants.
- **v12 RC2:** 740 real IPL-style matches over ten seasons plus 148 corrected-build matches over two further seasons, alongside 100 seeded fixture-fairness checks and 108 identical T20/ODI/Test inning comparisons.
- **v11.66 structural longevity:** 30 County Championship years and 3,780 **controlled results** testing progression and archived season identity—not full cricket simulations.
- **Cricsheet calibration:** v11.17 used 660 simulated ODI matches against a 66-match real current-rule ODI cohort. Earlier Test session/weather and specialty format suites added further focused simulations.
- **Save durability:** An in-memory 308-match/31-edition archive fidelity test passed; a roughly 504 MB older full-universe export **timed out**, and actual PC-to-PC restore remains a public playtest target.

**These tests span different versions and test types, so do not treat the figures as one cumulative match count or as certification of the exact Alpha 1.4 build.** The historical evidence audit covered 89/89 current private-repository branch trees and 68 available PR records, without running new tests. Calibrating realism is ongoing, especially 12/20-rated batting outliers and 9–10/20-rated bowling workloads.

[**Read the expanded September–November stress-test and calibration history**](./VALIDATION_HISTORY.md), including repeated-run cautions and unsuccessful tests.

## Alpha limitations

Keep independent backups. Real browser restart and PC-to-PC save restoration are not yet fully field-certified. The World Pool catalogue contains incomplete historic/new squads. Some specialty formats have incomplete P12E injury support. Custom PlanetCricket BBCode awaits installation by forum admins.

**[Download the game](https://github.com/MaqdoomAli/CricSim-Releases/releases/download/Simulator/CricSim_v12_Alpha1_4_Playtester.zip)** · [Read playtester guide](./README_PLAYTESTERS.md) · [Known issues](./KNOWN_ISSUES.md) · [Bug report](./REPORT_BUG.md)
