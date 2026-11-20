# CricSim — Development Stress Tests & Calibration History

**Scope:** Audited validation reports and archived project conversations from **September–November 2026**, spanning **different CricSim versions**, including the private `HFCricSim` repository and earlier calibration reports. This is a summary of past development evidence, **not** a claim that all these suites were rerun on public Alpha 1.4. Figures are not a single unique-match total: different versions, replays, synthetic schedules and focused regressions must not be blindly added together.

**Two distinct questions have guided our testing:**
1. **Engine integrity:** Does every ball, innings, dismissal, bowling figure, table, playoff, record and save agree with the game state?
2. **Cricket realism:** Do batters, bowlers, teams, weather, tactics and tournament outcomes behave credibly when compared against actual cricket?

Passing one does not prove the other.


## September: match-distribution and league-leader calibration

**This was the largest omission in the previous public summary.** Early CricSim calibration was not a handful of quick matches. It included entire seasons using built-in squads, and real Cricsheet outcomes as reference points.

| Version / source | Executed test | Findings and interpretation |
|---|---|---|
| **V16 — IPL/Cricsheet** | **100 full IPL seasons = 7,400 matches / 14,800 innings** | First-innings average **187.4** vs **188.5** in pooled IPL 2023–26; wickets **6.04** vs **6.26**. Problems still visible: **11.4% all-outs** vs **7.5%**, and **36.2% innings of 200+** vs **41.3%**. This justified subsequent work, not perfect-score claims |
| **V18 — IPL season-leader tails** | **Three independent 30-season runs = 90 seasons / 6,660 matches** | Orange Cap winning average **762.2 runs**; top-ten batters **622.0 runs**, compared with roughly **609** in the reference; **14.6 hundreds/season**. Purple Cap winner averaged **27.5 wickets**. Work targeted freak individual hot streaks and century rates without flattening high team totals |
| **V19 — ODI** | **1,200 deterministic ODIs**, four independent seeds (300 per seed) | Overall **5.771 RPO**; first innings **266.6**; 100+ scores **0.87/match**; 3+/4+/5+ wicket hauls **1.82 / 0.52 / 0.184 per match**. Comparison used ODI World Cup Cricsheet data, but the synthetic squad pool (India, Australia, Ireland, Zimbabwe) was **not matched** to World Cup quality |

**These are different engine versions and calibration phases**. Do not add V16 and V18 as if they were one unbroken long save, or treat V19's 1,200-match run as proof that all ODI leagues and years match reality.

The V16 research used **1,243 IPL matches (2008–2026)** and **265 men's ODI World Cup matches** as reference archives; those real matches are *research data*, not simulated matches. Its main correction was delivery texture: replacing implausible dot/four-heavy scoring with more credible singles and twos.

### v11.17–v11.22: protected format-specific changes

- **v11.17:** Ten independent replays of a 66-match post-July-2025 ODI cohort yielded **660 schedule-matched simulated ODIs**. A separately reconstructed **50 × 45-match tournament-leader guard** compared an older V22 engine with the newer ODI candidate; this is a comparator, **not** an independently verified actual IPL/World Cup calendar.
- **v11.19–v11.21:** Paired deterministic full-match regression pools of **630 IPL + 623 T20I + 213 ODI = 1,466 fixtures per version** checked that format-specific changes didn't disturb protected outcomes. These same fixture sets appear in multiple version reports; **count them once as a test family**, not once per mention.
- **v11.19 and v11.21:** Each additionally documented a **30-season, 2,100-fixture simplified IPL league guard**, specifically **70-match seasons without the real IPL playoffs**. Its purpose was identical-seed non-regression, *not* an additional 2,100-match historically calibrated IPL season audit.
- **v11.20:** **800 paired Test innings per version** examined second-new-ball batting/wicket behaviour. An additional 800-innings neutral/pitch mix, **320 paired Test innings** for first-80-over exact regression, and **450 paired synthetic dew scenarios** checked isolation. Venue/ground metadata coverage was incomplete.
- **v11.21:** **300 complete scripted rain matches**, **364 strict complete reduced-overs matches with 3,640 bowling/resource assertions**, **336 pre-rain exact-parity pairs** and **11 tournament/NRR scenarios** passed. Some scenarios and innings overlap across the rain validation methods; never silently present a summed unique-match count. The implementation uses published **Standard D/L**, not licensed professional DLS.

**Takeaway:** The earlier calibration already investigated averages, wickets, century and wicket-haul frequencies, phases, strike rotation, boundary distributions, partnership concentration, new-ball dynamics, dew, reduced-overs bowling quotas and chase pressure. It did not solve every problem.

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


## Further October/November stress families discovered in development branches

The repository has many source-pinned development branches that retain tests **not present on protected main**.

| Branch/report | Executed workload | Purpose and limitation |
|---|---|---|
| **v11.63 publishing** | **22/22 historical-boundary cases**, three T20/Test/FC same-seed match parities and **16 intermediate scorecard projections** | Chronological session/day commentary and spoiler-free scorecards; this was not a format-wide scoring calibration |
| **v11.71 rain/dismissal** | **64 real scripted rain matches / 128 innings**, plus **10 multi-interruption innings** | Bowling quotas, active wicketkeeper exclusion, wickets and scorecard fidelity |
| **v11.72 special laws** | **500 six-player exhibition matches**, 125 per tested ruleset | Experimental Sixes/Beach laws and scoring distributions; superseded in part by v11.73's 750-match suite, not independent evidence of modern injury coverage |
| **v11.76 commissioner matrix** | **376 competition configurations and 23,637 controlled/stubbed fixture outcomes** | Qualification and transaction logic. **No actual ball-by-ball cricket was simulated** in those 23,637 fixture outcomes |
| **v11.77 knockout planner** | **56 structural cases, 599 planned knockout fixtures** | Graph generation, seeding and byes; **no played cricket matches** |
| **v11.91 era-aware T20** | **900 synthetic first innings** (300 each for 2006, 2015, 2026), with 100 modern innings and 20 ODI innings used as no-change controls | Demonstrated distinct era scoring, not player/venue-matched historical IPL calibration |
| **v11.93 all-year benchmark** | **4,554 first innings** across 2004–2026 × T20/ODI/Test, **138 full-match smoke cases**, and **300 ledger-integrity innings** | Controlled same-XI era comparisons, not historically matched real scores |
| **v11.95 era recalibration** | **978 paired first-innings cases per version**, **69 full-match smoke cases**, **300 extra ledger checks** | Compared changed and unchanged distributions and protected format behaviour; candidate still had ODI dot-rate and historical-XI limitations |
| **v11.96 red-ball survival** | **180 real Test/FC matches per version**, spanning 2004/2015/2026 and 4/5-day durations, plus 300 innings ledger checks | Tested survival/draw sensitivity; 2004 four-day draws changed markedly, so this was held as experimental |
| **v11.97 Test captaincy** | Six 4/5-day conditions with **48 paired full matches each**, plus three 3-day conditions with 32 paired matches each and a 48-match dynamic-weather comparison | Detected default defensive-captain issue; XI depth materially altered draw rates, so historical draw calibration remained open |
| **v11.98 forecast declarations** | **264 paired complete red-ball matches per build**, plus 108 white-ball parity innings | Accepted narrow forecast-aware declaration changes, rejected an overbroad trailing-declaration ban that worsened draws |
| **v12 mixed-format Phase 2** | **Three six-team IPL-style editions (102 actual matches)**, plus 108 unchanged-engine seeded innings and mixed-format scenarios | Verified selected year, impact rules, advancement, records and playoffs; longer ten-team candidate timed out at that stage |
| **v12 RC4 bulk playtest** | **153 Vitality Blast fixtures** completed once in Chromium through Sim All | An independent repeated-run attempt **stalled/timed out**; not accepted as a long-running reliability pass |
| **P8.10 commentary** | **26 completed browser matches, 8,423 deliveries**, with **1,430/1,430 identical paired delivery tuples** in six T20 matches | Commentary attribution, delayed reveal and report data, not complete Windows durability |
| **P9.3 qualification** | A **729-outcome** four-team points-case starting space, reducing to **81 paths** in a test state; 14 UI navigation targets | Mathematical points routes and conservative fallback, not 729 played cricket matches |
| **P10.5 current-player cohort** | **120 full T20s, 100 ODIs, 80 Hundred matches, 36 Tests and 36 four-day FC games** | Real game functions with installed national player profiles in a headless harness; not real 2026 fixture/playing-XI matching |
| **P10.7 Hundred held-out study** | **408 2026 real-XI source-backed simulations per engine**, parent vs Hundred candidate (34 real fixtures as holdout) | Scoring/dot/rotation comparisons with historical ratings; **not** live browser/DRS/SQLite certification |
| **P12D injury checks** | 30/30 focused local controller/fielding/innings regressions and 3 red-ball smoke cases; separate **5,000 fixed-budget incidence draws per format** | The 5,000-draw T20/ODI/Test probability checks are **synthetic hazard tests, not 5,000 whole cricket matches** |
| **P11/RC9.1 desktop** | 42/42 targeted local gates at one candidate checkpoint, later native rollback and checkpoint unit suites | Source, browser fragments, save-transaction safety and migration guards. These do **not** certify a complete personal Windows profile after real cold restart |

### Cricsheet data audit (evidence gathering, not a game stress run)

In November, the P10.4 source audit parsed **23,133 Cricsheet JSON match files** and **50,954 non-super-over innings**, separating international/domestic competitions, men's/women's data, innings phases, shortened games, season and formats. These are **real source records, not CricSim-simulated matches**. Earlier P10.0 used a nearby but different archive snapshot of **23,132 files**; the one-record difference is not an extra simulated match. Local parser/cohort regression tests passed, but complete matched-competition calibration remained pending.

### Persistence nuance: a successful large synthetic browser restore

The earlier v11.68 development beta actually completed a **54-edition multipart browser export/restore of approximately 504 MB of highly repetitive synthetic commentary**, with browser-process close/restart and SHA-256 record comparisons, plus six saved Quick Matches. This was a genuine persistent-Chromium test, but **not** a restoration of the 998 real ball-by-ball matches. Its seven compressed synthetic parts totaled only about 1.76 MB; it doesn't establish normal 500 MB real-universe transfer speed or Alpha 1.4 cross-PC portability.

A historical October test ran on GitHub-hosted infrastructure before the later local-only testing policy. **No GitHub Actions tests were launched for this public-history update**; today's public pipeline synchronizes release metadata only.

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


## How to read this evidence honestly

- **Distinct complete-season runs:** V16's 100 IPL seasons, V18's 90 IPL seasons, the v12 ten-team RC2 editions and Alpha 1.3's 25 IPL seasons are separate dated engine candidates. Do not call all of them one uninterrupted save, or claim the complete exact public Alpha 1.4 build was retested at their combined scale.
- **Repeated regression pools:** A 1,466-match dry guard reported against v11.19, v11.20, v11.21 and v11.22 is the *same test family* replayed for consistency; it isn't 1,466 new matches each time for marketing purposes.
- **Paired A/B tests:** Tests per version/engine include *both candidate and parent* and may replay the same seed. Pair counts cannot be naively counted as independent cricket observations.
- **Controlled results / graph plans:** 23,637 stubbed fixtures, 3,780 County fixtures and 599 knockout-planner outputs prove structural behaviour, not actual ball delivery probabilities.
- **Real cricket observations:** Real Cricsheet records provide comparison targets; they aren't in-game matches.
- **Failures and caveats matter:** A 504 MB real-world-data-shaped bulk JSON export timed out in v11.66; real origin migration and complex save restoration have had separate limitations. Some corrections were deliberately rejected because they introduced worse cricket outcomes.

### Further public reading

- [Latest Alpha 1.3 IPL 25-season stress report](./IPL25_STRESS_REPORT.md)
- [Player guide](./README_PLAYTESTERS.md)
- [Known issues and unresolved acceptance gates](./KNOWN_ISSUES.md)

*Evidence basis: archived V16, V18, V19 and v11.17–v11.98 validation reports, v12 RC2/RC4, P8–P12 development branch acceptance notes, P10 Cricsheet audits, plus Alpha 1.3/1.4 playtest records. Historical source logs belong to development archives; this public document summarises their reported results and caveats rather than passing off old results as new Alpha 1.4 executions.*
