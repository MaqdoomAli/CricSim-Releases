# CricSim — Development Stress Tests & Calibration History

**Scope:** Representative documented runs from September–November 2026, across **different CricSim versions**. This is a summary of past development evidence, **not** a claim that all these suites were rerun on public Alpha 1.4. Figures are not a single unique-match total: different versions, replays, synthetic schedules and focused regressions must not be blindly added together.

**Two distinct questions have guided our testing:**
1. **Engine integrity:** Does every ball, innings, dismissal, bowling figure, table, playoff, record and save agree with the game state?
2. **Cricket realism:** Do batters, bowlers, teams, weather, tactics and tournament outcomes behave credibly when compared against actual cricket?

Passing one does not prove the other.

## Full ball-by-ball match and season stress tests

| Version / date | Executed workload | Main result | Scope or limitation |
|---|---|---|---|
| **v11.66 — 9 Oct 2026** | **998 real-engine matches**, **54 tournament editions**, **2,081 innings**, **313,975 legal balls** (**323,316 recorded deliveries**) | No discrepancies in the audited scorebook, extras, legal-ball, bowling, fall-of-wickets, table or reconstructed player-stat checks; no Chromium exceptions reported | Synthetic small leagues using real international squads, not historically matched IPL/other schedules. The giant full-universe export **timed out** separately |
| **v11.66 scripted-weather extension** | **42 further real T20/ODI matches** in four editions, including **4 no-results** | Scorecards, ball ledgers, extras and tournament-stat reconstruction reconciled | Tests weather pathways, not the natural annual probability of rain |
| **v12 Phase 2 / RC2 — 15 Oct 2026** | **10 complete ten-team IPL-style editions: 740 matches**, followed by **2 fixed-candidate editions: 148 matches** (**888 real-engine games** in the audit) | All editions finished with league standings, four playoff fixtures, winners and recorded individual statistics | An earlier fixture generator was found to give extreme home/away imbalance, then corrected; the versions and 740/148 subsets must stay distinct |
| **Alpha 1.3 — 20 Nov 2026** | **25 complete IPL seasons, 1,850 matches**, including **100 playoff matches** | **Zero simulation exceptions** in this run; breakout player, ratings, bowling-usage and medical-event checks recorded | Relevant recent gameplay candidate, but not proof that every different format, save migration and forum deployment is production-certified |

### The six-format v11.66 stress audit in detail

| Format | Matches | Editions | Legal deliveries |
|---|---:|---:|---:|
| T20 | 560 | 20 | 124,763 |
| T10 | 120 | 8 | 13,373 |
| The Hundred | 120 | 8 | 22,881 |
| ODI | 150 | 10 | 78,792 |
| Test | 24 | 4 | 36,971 |
| Four-day first-class | 24 | 4 | 37,195 |
| **Total** | **998** | **54** | **313,975** |

The audited innings also contained **17,813 extras** and saved scorecards, including draws and ties. The report checked runs = batter runs + extras, legal-ball counts, bowling-figure reconciliation, falls of wickets, standings and reconstructed player totals.

## Tournament logic, long-term seasons and fixture fairness

These are crucial tests, but **controlled-fixture tests are not ball-by-ball match simulations**:

- **v11.66:** All seven built-in domestic competition graphs completed through **576 controlled fixture outcomes**, verifying qualifiers, standings and final champions.
- **v11.66:** **30 consecutive County Championship editions** went through **3,780 controlled fixtures** (126 per edition), exercising season-to-season identity, divisions, promotion/relegation, standings and serialization of the archived editions. These results were intentionally controlled, not live match scorecards.
- **v12 RC2:** **100 seeded ten-team fixture generations** verified fourteen league matches per team, balanced **7 home / 7 away**, correct opponent coverage and no duplicated fixture IDs.
- **v12 RC2:** **108/108** actual T20, ODI and Test innings matched the pre-fix baseline after the home/away-only patch, demonstrating that the underlying ball engine had not silently changed.
- Later multi-stage tournament and qualification tests examined graph dependencies, no-result progression, mixed-format sections, stage transitions and mathematically possible qualification paths. Their targeted acceptance checks supplement the full-season runs; they are not additional 1,000-match simulations.

**Why this matters:** Previous stress tests uncovered *real defects*. The ten-team run exposed a home/away imbalance as extreme as thirteen home games versus one away game before the generator was fixed; a large archive test exposed a serious performance bottleneck. Finding and fixing faults is as important as reporting passes.

## Real-cricket calibration and match-rules testing

The simulator was compared with supplied **Cricsheet ball-by-ball records** rather than calibrated solely on feel. Representative documented work:

- **Modern ODI calibration, v11.17:** Used **66 real current-rule ODIs** as a target and ran **660 schedule-matched simulations** over ten seed families. The **real first-innings mean was 251.92**, versus **248.57** in the recalibrated simulator (238.27 in the prior version). Chase win rate: **54.55% real** versus **56.82% simulated**; first-innings all-outs: **42.42% real** versus **46.67% simulated**. That is improved agreement, **not perfect calibration**.
- **Test-session engine, v11.33:** **100 paired seeded Tests** compared tactical behaviour before and after session-aware decisions; a separate **24/24 complete dynamic-weather Tests** finished with **zero engine errors** and **83 retained tactical events**. Dry T20 and ODI seeded regressions remained unchanged.
- **Weather-rule stress:** Thousands of seeded automatic rain plans, reserve-day checks, rain/snow probability boundary cases and dry-match parity checks were run in the earlier v11.29–v11.31 series. Individual scenarios and statistics were tested rather than claiming the model is the real forecasting system.
- **Six-player exhibition engines, v11.73:** A separate **750-match seeded specialty-format suite** exercised Hong Kong Sixes and Beach Cricket score/ledger behaviour. These experimental formats are **not** proof of complete injury support in Alpha 1.4, and their historical-score calibration gate was intentionally loose.
- **Alpha 1.3 IPL breakout audit:** Some **12/20** batters produced exceptional seasons and some **9–10/20** part-time bowlers received substantial workloads. Those outliers are being investigated; we should not claim every rating is perfectly balanced.

## Persistence, backups and release validation

- **v11.66 large-universe audit:** The 998-match game simulation completed, but attempting an approximately **504 MB uncompressed full-universe JSON pack** exceeded an external **180-second timeout**. The successful matches and failed bulk export were different outcomes.
- **v11.67 compressed-backup beta:** A **308-match, 31-edition** real-game archive, roughly **127.81 MiB** in source form, encoded to roughly **14.01 MiB**, then decoded and compared. This was an **in-memory record-fidelity** result, not a fully proven cold-browser restore.
- **Alpha 1.4 portable-save tests:** Focused snapshots, integrity checks, retention and a five-fixture live IPL integration sample verified that played matches and statistics were included in exported game data. **Physical PC-to-PC recovery, permission renewal and very large long-running universes still need public playtesting.**
- **Release delivery:** The public Alpha 1.4 ZIP is independently downloadable from GitHub with its SHA-256 recorded in the public release. The custom PlanetCricket forum styling depends on administrator installation and isn't certified by simulation tests.

## What these results do—and do not—prove

**They support:** Extensive work on scorebook consistency, multiple formats, tournament completion, fixture fairness, real-ball match execution, statistical calibration, save-fidelity testing and rare-rule regressions. Many tests were repeated because features, rules, structures and source builds changed.

**They do not prove:** That all earlier historical cases passed again on the **exact Alpha 1.4 public build**, that all Cricsheet distributions match perfectly, that every historical squad is accurate, that every optional special format shares the same injury system, or that a genuinely large save has been copied to another PC and restored under all conditions.

We are publishing this as an **alpha** precisely because we want independent players to expose the next set of issues.

---

### Further public reading

- [Latest Alpha 1.3 IPL 25-season stress report](./IPL25_STRESS_REPORT.md)
- [Player guide](./README_PLAYTESTERS.md)
- [Known issues and unresolved acceptance gates](./KNOWN_ISSUES.md)

*Evidence basis: dated v11.17, v11.33, v11.66, v11.67, v11.73 and v12 RC2 development validation reports, plus Alpha 1.3/1.4 playtest notes. Historical source logs belong to development archives; this public document summarises their reported results and caveats rather than passing off old results as new Alpha 1.4 executions.*
