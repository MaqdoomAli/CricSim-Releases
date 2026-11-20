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

- **v11.66:** 998 genuine ball-by-ball matches across T20, T10, Hundred, ODI, Test and FC (54 tournament editions, 313,975 legal balls), with no reported mismatches in the audited score/stat invariants.
- **v12 RC2:** 740 real IPL-style matches over ten seasons plus 148 corrected-build matches over two further seasons, alongside 100 seeded fixture-fairness checks and 108 identical T20/ODI/Test inning comparisons.
- **v11.66 structural longevity:** 30 County Championship years and 3,780 **controlled results** testing progression and archived season identity—not full cricket simulations.
- **Cricsheet calibration:** v11.17 used 660 simulated ODI matches against a 66-match real current-rule ODI cohort. Earlier Test session/weather and specialty format suites added further focused simulations.
- **Save durability:** An in-memory 308-match/31-edition archive fidelity test passed; a roughly 504 MB older full-universe export **timed out**, and actual PC-to-PC restore remains a public playtest target.

**These tests span different versions and test types, so do not treat the figures as one cumulative match count or as certification of the exact Alpha 1.4 build.** Calibrating realism is ongoing, especially 12/20-rated batting outliers and 9–10/20-rated bowling workloads.

[**Read the full version-by-version stress-test and calibration history**](./VALIDATION_HISTORY.md).

## Alpha limitations

Keep independent backups. Real browser restart and PC-to-PC save restoration are not yet fully field-certified. The World Pool catalogue contains incomplete historic/new squads. Some specialty formats have incomplete P12E injury support. Custom PlanetCricket BBCode awaits installation by forum admins.

**[Download the game](https://github.com/MaqdoomAli/CricSim-Releases/releases/download/Simulator/CricSim_v12_Alpha1_4_Playtester.zip)** · [Read playtester guide](./README_PLAYTESTERS.md) · [Known issues](./KNOWN_ISSUES.md) · [Bug report](./REPORT_BUG.md)
