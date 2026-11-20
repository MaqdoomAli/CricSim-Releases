# CricSim — From the First Simulator to Alpha 1.4
## A feature-by-feature development history | May–20 November 2026

**Starting point:** An existing offline cricket simulator actively expanded by 1 June 2026, with a dated v11.6 IPL 2026 HTML snapshot.  
**Public milestone:** CricSim v12 Alpha 1.4, the first public PC playtest, released as a self-contained HTML game for desktop Chrome/Edge.  
**This document:** What changed, why we changed it, what was retained or replaced, and which branches remained experiments.

> **Dating note:** The developer reports starting work in **May 2026**, but the exact date, file and feature set of the *very first* prototype have not been recovered. We have an inspected **v11.6 IPL 2026** HTML snapshot archived as 1 June, a modular-source archive dated July, another source snapshot in August, dated September validation reports, and much stronger source/branch records from October onward. Dates below represent the earliest documented milestone or report, **not necessarily the day a feature was first conceived or written**.
>
> **Release note:** A feature appearing in an experimental branch does **not** automatically mean it shipped in Alpha 1.4. Status is explicitly distinguished below. The first public “PC release” is an **offline HTML/JavaScript application opened in a PC browser**, not a certified native Windows EXE.

---

## The journey in one picture

| Stage | Period | What CricSim was becoming |
| --- | --- | --- |
| **The foundation** | May–June | A standalone cricket simulation with matches, team/squad control, conditions, tournaments and records |
| **A system rather than a script** | July–August | A growing source architecture; world-cricket competition research; modular code and plans to preserve saves and rules |
| **Make the cricket believable** | September | Cricsheet-informed, ball-level calibration; IPL/ODI season distributions; authentic bowling, venues and Test phases |
| **Make the cricket governable** | Late September–early October | Better captaincy, XIs, rain/D/L, match laws, player identities, domestic structures and multi-edition tournament persistence |
| **Make the universe durable and publishable** | 7–15 October | Reliable scorecards, interval reports, graph safety, massive backup experiments, mixed-format series, custom competition tooling, readable UI |
| **Make it a complete application** | 15–31 October | v12 integration, modern squad hub, full Match Centre, DRS integration, forum publisher, editorial commentary, qualification intelligence, desktop experiments |
| **Prove and prepare it** | 1–18 November | Qualification scenarios, real-XI/Cricsheet research, save migration/rollback protection, retirement and match-day injuries, release acceptance |
| **Share it** | 18–20 November | Alpha 1.x releases, PlanetCricket export styles, medical rules in normal tournament formats, IPL season stress and Alpha 1.4 portable save folder |

The important change wasn't simply “more features.” CricSim moved from **simulating one match** to managing a **persistent, editable cricket world** in which squads, playing conditions, tournaments, scorecards, statistics and match histories are connected.

---

## Chapter 1 — May to June: the working cricket simulator

**May 2026 — Beginnings (owner-reported, exact prototype unverified).** Work begins on a self-contained cricket simulator. No verified May source snapshot has been recovered, so this history does **not** invent a v1 release date or a precise May feature checklist.

**1 June — A real working v11.6 build already existed.** The archived `CricSimv11_6_IPL2026_v3.html` is an actual offline HTML application, not just a proposal. Its navigation already contains **Quick Match, Tournament, Teams & Squads, Archetypes, Match History, All-time Statistics and Documentation**. Quick Match exposes T20, ODI, Test, T10, The Hundred and custom overs, pitch, weather, toss, named team/squad selection and quick presets. The Tournament homepage advertises League, Knockout, IPL, World Cup and Swiss competitions. Batters and bowlers have editable **separate 1–20 skills**, archetypes and pace/spin style. The home screen documents an earlier v10.1 **unified Pitch×BowlerType outcome distribution** that replaced an older two-pass scoring/wicket model. This tells us the June game was already substantial—and that **we were improving an existing simulator, not starting a match engine from a blank file**.

**1–2 June — Your first documented product constraints.** The conversation says to improve the *acquired/existing simulator* rather than replace it. You specified that a commissioner must be able to **manually edit player ratings/skills during seasons and tournaments**, while **Test, ODI and T20** behaviour should be audited rather than assumed. On 2 June you explicitly rejected **automatic ageing, progression, regression, retirement and automatic rating changes**. Tournament statistics had to be **distinct from career/all-time totals**, with cross-edition aggregation optional and transparent.

**A revealing June storage limitation:** The v11.6 source kept `teams`, `matches`, `pstats` and `tournaments` in browser `localStorage` (`cs4_*` keys), and wrote only the **latest 100** Quick Match records when saving. That primitive, browser-profile-bound persistence is part of why months later we were building IndexedDB migration, full-universe exports and portable save-folder snapshots.

**The starting cricket engine was already opinionated:** The archived v11.6 home page describes pitch-specific probability tables with outcomes [dot, 1, 2, 3, 4, 6, wicket], 1–20 player skills influencing outcome weights, automatic pace/spin attack selection, chase urgency, and corrected run-out end logic. A newly created player could already be a Basher, Dasher, Technician, Anchor, Blocker or Pinch Hitter. These are **baseline capabilities visible in the June file**, not September inventions.

That foundation set decisions that still shape the release:

- Ordinary gameplay should be **portable and offline**, not dependent on a hosted server.
- The manager should be able to **edit players, squads and competition rules** rather than accept a permanently frozen roster.
- The match engine, tournament state, scorecards and statistics should agree on what happened.
- Refactoring or adding a PC host must **not destroy working saves or rebuild cricket mechanics from scratch**.

**What is not known:** The first prototype from May, the exact date the inherited engine's v10.1 and v11.6 changes were written, and which of the June controls already had comprehensive regression coverage. The June v11.6 artifact demonstrates early **screen-level features** but is not proof every format or tournament path was already correct. An earlier architecture proposal divided the project into engine, formats, captaincy, tournaments, roster/rating data, venues, statistics, commentary, interface, history and testing; that proposed folder plan is evidence of **design direction**, not proof each part was implemented in June.

## Chapter 2 — July to August: from one game to a world-cricket system

**19 July — World cricket beyond quick matches.** An archived “World Cricket Research Project” started mapping national and domestic competition structures, governing bodies, relationships, current and historical competitions. This was **reference research**, not a claim that every competition was instantly playable. It anticipated later domestic templates, multi-stage qualification and the international World Pool.

**19 July — World Cricket Research Volume 1.** A separately maintained research volume focused first on ICC governance, Indian cricket administration, domestic and franchise competitions, state associations and the cricket development pathway. You wanted **one continuous, carefully researched document**, not a premature database or a parade of disconnected proposals; this informed later World Pool and domestic-competition research but was not itself a playable feature.

**19 July — Modular source archive.** A separate archival inventory records a **cricsim-modular-source.zip** dated 19 July 2026. By then the project had outgrown a few ad-hoc scripts and was being organised for incremental changes.

**26 August — Revised modular source.** The same inventory records a **cricsim-modular-source_PATCHED.zip** dated 26 August. This shows an active engineering/source lineage, but the inventory alone does **not** identify a specific new August gameplay mechanic. Avoid converting file-modification dates into invented release claims.

**Architectural direction:** Preserve the standalone HTML as the *playable build*, while splitting its source into inspectable pieces with checksums and regression tests. This later became an important defence against accidentally changing the scoring engine while fixing commentary, presentation or data.

---

## Chapter 3 — September: stop judging realism by the final score alone

Early calibration showed that an innings could have a reasonable average but **the wrong kind of cricket inside it**. Too many dots and boundaries, too few singles, overly fragile tailenders and implausible season leaders could hide underneath attractive overall totals.

**V16 — Actual delivery texture and IPL seasons.** Using the supplied IPL Cricsheet archive, the simulator was tuned against legal-ball outcome mix: dots, singles, twos, boundaries, wicket rates and phase patterns, not only total runs. A documented **100-season / 7,400-match** IPL run checked the result. It remained imperfect—especially all-outs and 200+ innings—but improved rotation and tail behaviour.

**V17/V18 — Team avalanches are not the same as immortal batters.** The IPL leader-tail work distinguished a team scoring 240+ from a single batter unrealistically making huge scores too often. **V18** underwent **90 full IPL seasons / 6,660 matches** examining Orange/Purple Cap totals, centuries, strike rates, high-score tails and bowling hauls.

**V19–V22 — ODI is a separate problem.** ODI work used 2019 and 2023 World Cup ball-by-ball data as its modern reference, kept older editions for era context, and separately calibrated opening, middle and closing overs. Follow-on versions addressed XI/team quality, weaker middle-order survival, bowling at the death, last-pair behaviour, the relative frequency of 300/350+ scores and 150+ individual innings. **V22** was validated over **30 45-match ODI tournaments**. Several seemingly easy global scoring fixes were rejected because they damaged the distribution elsewhere.

**Tests also forced attention onto red-ball cricket.** A provisional Test audit measured declarations, draws, follow-ons, innings lengths and wickets, rather than using T20 or ODI assumptions. This line later drove the session clock, Test captaincy and three-/four-/five-day calibration work.

**29 September — v11.13 Test venues and home advantage.** Test ground pools and more national home-ground lists were introduced or corrected, including region-aware weather and surfaces. The goal was to stop treating Galle, Perth and Lord's as interchangeable without inventing pitch facts unsupported by available data.

**30 September — v11.20 venue/day-night context and the second new ball.** Named grounds and day/night context were threaded into eligible match situations; the Test second-new-ball wicket/scoring effect was audited separately. Dry-match regressions protected unrelated formats.

**The lesson of September:** A realistic simulator needs separate answers for (1) who plays, (2) where and under which rules, (3) what happens *on each delivery*, and (4) what a whole season looks like.

---

## Chapter 4 — 1–8 October: cricket laws, squad intelligence and domestic structure

**1–2 October — Rain and interruptions mature (v11.21 → v11.28).** Scripted reductions for T20/ODI were built around published **Standard Duckworth/Lewis**, minimum overs, bowling quota reassignment and innings continuity. Multiple interruptions and optional automatic weather arrived by v11.28, with separately preserved **full Super Over delivery records**. The professional licensed DLS calculator was **not** implemented. Automatic weather remained opt-in.

**Early October — Publishing starts becoming a product (v11.22–v11.27).** Full match stories, chronological BBCode, standalone HTML reports and PlanetCricket-oriented formatting evolved. Commentary should precede the innings scorecard it explains; a result or future match detail must not be disclosed before the audience reaches it. Later work polished styling, but this **spoiler and scorecard authority** decision dates back to the early publisher.

**v11.34–v11.36 — Selection and bowling are tactical systems.** MatchCaptain gained bowling phase/deployment preferences, backup eligibility and more consistent workloads. **Shall Bowl** and **Can Bowl** let a manager distinguish the preferred attack from emergency options. Quick Match and tournament **Auto XI** logic were unified. The bulk-simulation path was fixed to carry manager bowling intent and count playoff/knockout stats, rather than silently producing different XIs or missing player numbers.

**2 October — v11.37 separates a person from a performance version.** One player's identity became distinct from their **T20/ODI/Test, era-specific Performance Profiles**, and from **Squad Membership**. That allowed a player to represent multiple teams and eras without inventing multiple people. A Players interface and migration path for older squads followed. Imported namesakes could be split instead of being merged forever by matching name strings.

**Around 7 October — Compressed World Pool and domestic templates (v11.40–v11.42 / Phase 2T–2U).** The embedded source library grew to thousands of player identities and profile/squad snapshots, while domestic competitions acquired distinct structures instead of an IPL-shaped generic league. The Phase 2 domestic registry held **30 templates, 87 stages and 690 declared fixtures**. Phase 2U used controlled pitch matrices to correct surfaces that behaved backwards, and tested real season examples such as the One-Day Cup, BPL and Sri Lankan first-class cricket.

**8 October — Tournament family versus tournament edition (Phase 3A–3C / v11.45).** A league could have **multiple independent historical editions**. The next edition could change from five to fifteen teams, add/drop archived teams, create a fresh fixture/rules graph and retain earlier players and results. An old player's removal from an active edition did not rewrite the archived edition. Universe backup needed to preserve that entire family.

**Early October — Memory and storytelling (v11.59 onward).** Match commentary and match stories began taking limited, verifiable context from earlier fixtures in the **same edition**, then carefully bounded previous editions (former champions, relevant rivalries, record holders). Unrelated tournaments, Quick Matches and future results were deliberately excluded.

The big conceptual advance was that **a tournament ceased being a disposable list of fixtures**. It became a history with teams, players, editions, standings and genuine consequences.

---

## Chapter 5 — 9–12 October: a stable baseline, then many independent experiments

**9 October — v11.63: publish a Test by session or by day.** Once a Test/FC match finished, CricSim could export a lunch/tea/stumps session or full day's report with only **events already played by that time**, plus a mathematically reconstructed provisional scorecard. This did **not** pause and resume the simulation; it replayed the saved delivery ledger without revealing future wickets or runs.

**v11.64: narrowly audited commissioner corrections.** A genuine mistake in a recently completed eligible league match could be corrected **once** by changing the actual XI/bowling permissions and supplying a reason. The original result and audit trail remained. The ordinary user was not given unlimited Undo/Resimulate, and complex dependent tournaments/long formats were not silently reflowed.

**v11.65: broadcast commentary grows more contextual.** Text shifted away from repetitive generic calls toward recorded score pressure, partnerships, wickets, phase and session context. The rule was non-negotiable: **presentation must consume no extra scoring RNG** and cannot fabricate a fielder or a boundary.

**v11.66: the stable fallback.** Tournament graph integrity was hardened after an actual bug silently dropped entrants from an invalid finals stage. Preflight began refusing broken bracket edges, duplicate qualifiers and premature finals. Division statistics were restored when older fixtures carried a generic stage ID. A six-format 998-match stress audit and long structural tournament tests supported the build. **v11.66 remained the historical stable fallback while later candidates were trialled.**

### Experimental work that branched from the stable base

| Approximate date / version | New feature or correction | Release qualification |
| --- | --- | --- |
| **9 Oct, v11.67–v11.68** | Lossless compressed, multipart **full-universe backup**; IndexedDB-sourced streaming and atomic restore. A large, deliberately repetitive synthetic universe was restored after browser-process restart | Beta persistence experiments; not proof of Alpha 1.4 cross-PC restoration |
| **9 Oct, v11.69–v11.71** | Independent match-ledger auditor, XI preflight, correction UX, proper Hundred/T10 bowling limits, active-keeper fielding restrictions, improved rain bowling allocation, spoiler-free reports | Isolated candidates with tested fixes; later source integration had to preserve exact cricket outputs |
| **9 Oct, v11.72–v11.73** | Experimental **Hong Kong Sixes and Beach Cricket** rule engines, specialised scorecards, runner/strike and boundary handling, illustrated beach-field assumptions | Different from full-XI modes; historical scoring coverage sparse; not a completed injury system |
| **9 Oct, v11.74–v11.75** | A **mixed-format bilateral touring series**, followed by 3–20-team mixed-format groups and playoffs, fixture-specific format/rules and cross-format result contracts | Experimental commissioner tools; all-format stats separated rather than inventing a single NRR |
| **9–10 Oct, v11.76–v11.78** | Qualification integrity for asymmetric groups, bracket/byes planner and a guided **Visual Architect** using actual teams, named groups and explicit qualification edges | Structural/mocked-DOM gates must not be mistaken for completed browser playthroughs |
| **10–11 Oct, v11.79–v11.82** | Searchable/paged team discovery and **hybrid/Broadcast/Pavilion/Studio** UI experiments, replacing giant scrolling lists and obscure tournament configuration | Several UI alternatives/candidates; no claim every early theme was shipped |
| **11–13 Oct, v11.83–v11.90** | Source-aware **player-name, batting-hand, bowling-action and keeper-role** corrections; “unknown” metadata rather than invented right-handed seamers | Some identities were verified; a proposed 400-player all-or-nothing queue was superseded by smaller evidence batches, not claimed as 400 corrected players |

This period established **change isolation**: source chapters and manifest checks rather than casually pasting over a multi-megabyte HTML. The developer tree eventually held **64 separately inspected/editable source chapters** while the product remained a standalone file.

---

## Chapter 6 — 14–18 October: history, fantasy and the v12 integration

**14–15 October — v11.91–v11.98: selected year becomes real match context.** Earlier historical player profiles alone did not make a 2006 T20 play differently from a 2026 T20. An era-aware engine began using the selected year to change ball-level patterns, with separate T20, ODI and Test/FC calibration paths and three-/four-/five-day checks.

Crucially, we separated **simulation year** from **commissioner-selected fantasy rules**. Someone could stage a fictional 2006 contest under a modern rule variation without the game silently rewriting their players' skills or declaring it “historically impossible.” ODI-era calibration remained incomplete in an early v11.94 candidate; later changes were deliberately conservative.

**15 October — v12 integration (RC1/RC2).** Previously separate source chapters and competition pathways were recomposed into a full playable candidate. An actual duplicate JavaScript declaration was caught; **51 script blocks** had to coexist. Mixed-format series and tournaments received one authoritative year setting, optional impact/free-hit policies and frozen per-fixture rule snapshots. Multiple linked six-team editions could finish without rewriting prior seasons. Browser save migration required transaction completion before clearing old keys.

**15–18 October — RC3 and RC4: playtest hardening.** Fair fixture schedules for 4–20 teams, protected domestic/franchise roster references, a **current-squads-first** browsing experience, team/fixture filtering, tournament creation fixes and a redesigned fixture/results centre were pursued. The ten-team IPL run exposed an extreme home/away imbalance, which was corrected and regression-tested. Sim All gained checkpoint and progress work after slow/uncertain full-competition runs.

**17 October — realistic roster dates take priority over reconstructing every old season.** The World Pool/Competition Atlas began prioritising present-day squads, current identities and recent-year fallback data when an exact 2026/27 roster was not available. Some older squads were explicitly labelled source-only or provisional instead of being falsely advertised as current. Correct role metadata could change the bowling engine; player batting/bowling **skill numbers were not arbitrarily altered to match a preferred result**.

**18 October — a valuable warning from RC4.** One 153-fixture Vitality Blast Sim All button test completed in Chromium, but repeated browser attempts stalled. That was a reliability issue, not a reason to claim a fully certified bulk simulator.

**The result of these experiments:** A simulator that knew *which cricket world* it was supposed to be modelling—not simply which two shirts were on the field.

---

## Chapter 7 — 18–31 October: the Match Centre, DRS and readable cricket

**18–24 October — RC4 → RC8.8.** The focus shifted from back-end models to what a player actually sees: fixture lists, tournament views, live result presentation, recovery checkpoints, and a consistent **Match Centre**. RC6/RC7 introduced stricter save/transaction guards. RC8–RC8.8 integrated more of the scorecard, result, match commentary and **Decision Review System (DRS)** playtest work.

DRS grew from an isolated rules prototype into user options including **off, LBW, caught-behind/bat-pad and restricted review modes**, unsuccessful-review budgets and match/format settings. Reviews needed to appear at the correct delivery in commentary and scorecards. It had to decide a ball **before the result was committed**, rather than editing a finished scoreboard casually. Some early physics/appeal experiments were provisional; present DRS isn't a claim of a full 3D ball-tracking physics engine.

**24–30 October — commentary and editorial P8 series.** The user wanted every ball to show the basics plainly—**ball number, bowler to batter, outcome and meaningful narrative**—without limiting it to one formulaic line. P8.5–P8.10 explored chronological reveal, detailed situational descriptions, bowler changes, run-out attribution, fewer exact repeated phrases and correctly placed review notes. Some candidate wording was held back because the engine didn't physically record enough contact/shot geometry to substantiate it. The final principle remained: the scoreboard is authoritative; descriptions cannot make up physics.

**25–30 October — statistics and Tournament Hub review.** The app's extensive page/navigation surfaces, batting and bowling statistics, squad views, records, fixtures and table presentation received audits. A broken bowling view and inconsistent content sparked cross-page checking, not just a cosmetic patch.

**23 October onward — Windows native host experiment (RC9).** A desktop host reused the existing HTML/JavaScript engine instead of rewriting cricket logic. Go/SQLite persistence, migration, backups and process-level recovery were explored. This was a **separate native EXE experiment**; it did **not** become the certified Alpha 1.4 player download.

---

## Chapter 8 — 30 October to 13 November: intelligent competitions and release gates

**30 October–2 November — P9.3 Qualification Analyst.** A read-only Tournament Hub could enumerate possible **points-only** futures in supported small leagues, distinguish **secure / still possible / tiebreaker-dependent / mathematically eliminated**, and respect multi-stage or combined championship tables. It refused to guess NRR guarantees, future scores or unknown fixtures. This was one of the clearest steps from a simulator to a **competition-management game**.

**3–13 November — P10: measure the right player, rating and format.** Research traced broken format-rating import paths, compared game outcomes to **real Cricsheet cohorts**, checked national playing XIs and examined The Hundred using source-backed actual-XI comparisons. The findings implicated **ratings, squad participation and captain deployment**, not just random batting weights. We deliberately did **not** solve inaccurate player usage by blanket 1–20 skill nerfs or unverified historic squad guesses.

**Early–mid November — P9.4 Decision Centre.** Building on P9.3, the Tournament Hub could identify **which remaining fixtures matter** and conditional routes a selected team needs. This was still a bounded, mathematically grounded view—not a bookmaker's probability calculator or invented projected NRR.

**Early–mid November — RC9.1 save migration safeguards.** The desktop pilot developed a guided **full-universe JSON → SQLite** migration with explicit validation and consent, plus rollback and readback checks. Real Windows player-universe cold restart was still an open acceptance gate. The lesson directly informed the simpler **user-selected portable save-folder** feature that eventually shipped in the browser edition.

**13 November — P11 playtest acceptance discipline.** The release plan began insisting on **actual game actions**, multi-format match runs, stored results, restored data, browser panels and crash protection—not just syntax checks or green unit-test counts. The exact full Windows native app remained unverified, so the public first release stayed HTML.

---

## Chapter 9 — 13–20 November: injury rules, forum sharing and the first public alpha

**13–18 November — P12 retired hurt/out and bowler availability.** A long-standing missing cricket law was added: **retired hurt is not a dismissal**, tactical **retired out is a dismissal without bowler credit**, an eligible hurt batter may return in the same innings under the selected rules, and a hurt bowler's workload must be completed lawfully.

We kept this **lightweight and realistic** rather than introducing random career-ending injuries. If the whole squad contains **only eleven players**, spontaneous bowler injuries are skipped; a batter can still be hurt. With a genuine reserve, CricSim can select a substitute fielder rather than invent a 12th player or grant that reserve batting/bowling rights. Normal XI members must cover the legal bowling. Injuries remain optional and deliberately rare.

**18 November — Alpha 1.1 candidate.** The P12E controls entered a playable standalone HTML, initially centred on Quick Match. Earlier unintegrated P12 branches were superseded after Test/FC return/carryover corrections. This is an important example of **one feature passing unit tests but not yet reaching every gameplay path**.

**19 November — Alpha 1.2: PlanetCricket presentation.** Reports, squad exports, fixtures, standings and statistics gained a more unified publisher with optional accent colours and user-specified HEX codes. The **enhanced custom BBCode** was submitted for forum administrator installation. It is **not** packaged as an admin ZIP in the public game release and **does not work until PlanetCricket installs the required tags/styles**. A standard BBCode fallback remains.

**20 November — Alpha 1.3: injuries across standard tournament modes.** The care system was carried into the general **Quick Match and tournament/full-XI paths**, including T20, ODI, T10, The Hundred, Test and FC. The actual IPL tournament engine completed **25 complete seasons / 1,850 matches** with a small number of physical incidents. This revealed remaining player-rating/bowling-usage questions rather than proving perfect calibration. **Hong Kong Sixes and Beach Cricket still have incomplete P12 injury integration.**

**20 November — Alpha 1.4: take your whole universe with you.** The player can choose a **Portable Save Folder**, write verified full-universe snapshots, keep rolling recovery copies, and copy the directory to another PC. The existing browser-local state and **Universe JSON import/export** remain as fallbacks. Saving the world matters as much as simulating it: teams, edited players, tournaments, fixtures, match results, scorecards and statistics should follow the player.

**20 November — first public PC playtest.** The public-facing **CricSim-Releases** repository and release distribute a **standalone HTML game plus playtester documentation**. The private developer repository remains private; a separate PlanetCricket administrator package and experimental native EXEs are **not** part of the public player ZIP. The first public release is explicitly **Alpha**, because independent Windows browser saves/restarts, real PC-to-PC transfer, long-running universes and some special-mode rules still need broader field testing.

---

## How the major features evolved

| Feature family | First documented foundation | How it changed | Position at Alpha 1.4 |
| --- | --- | --- | --- |
| **Ball-by-ball match simulation** | Working by June | Cricsheet-scored phases, realistic rotation/tail, format-specific tactics and accounting invariants | Core shipped |
| **Match formats** | Multi-format foundation by June | T20, ODI, Test, FC, later T10, Hundred, custom overs and experimental specialty rulepacks | Standard formats shipped; Sixes/Beach separate and not medical-complete |
| **Captaincy/lineups** | Early XI and bowling AI | Shall/Can Bowl, bowling phase roles, unified Quick/Tournament selection, sensible emergency cover | Core shipped; further part-timer allocation calibration needed |
| **Players and teams** | Editable squads and ratings | Stable identity → profile → membership, World Pool, source provenance, present-day squad focus | Core shipped; historical/current roster completeness varies |
| **Tournament builder** | Leagues and tournaments | Domestic stage graphs, edition histories, customizable mixed-format groups, brackets, fixtures, commissioner rules | Broad tools shipped; bespoke/unsupported graph edges still need fail-closed handling |
| **Match conditions** | Pitch, venue, weather | Home grounds, second new ball, year-specific scoring, rain/D/L, optional DRS, free hits, Impact Player | Broad controls shipped; Standard D/L rather than licensed Professional DLS |
| **Commentary/reports** | Scorecards and commentary | Chronological match story, Test session/day packets, Match Centre, spoiler-safe ball reveal, improved wording | Player-facing reporting shipped; generated descriptions bounded by available match facts |
| **Statistics and analysis** | Player/team stats | Tournament leaders, historical editions, scoped division stats, records, Qualification Analyst / Decision Centre | Shipped feature family; playtesters asked to report inconsistent tables |
| **Sharing** | Basic text and forum exports | Styled PlanetCricket match/squad/table/fixture/stats publishers, colour choice and normal BBCode fallback | Normal sharing shipped; richer forum rendering awaits admin |
| **Saving** | Browser-local persistent universe | Large compressed-backup experiments, safe importing, SQLite native pilot, rolling portable snapshots | Alpha 1.4 portable folder shipped; true cold restart/cross-PC validation ongoing |
| **Injuries/retirements** | Previously missing | ICC/MCC rules, return eligibility, reserve fielders and bowling cover, restrained randomness | Shipped for standard full-XI paths; specialty modes incomplete |
| **Native desktop EXE** | October RC9 pilot | Existing HTML hosted with Go/SQLite, migration and recovery experiments | **Not** the public Alpha 1.4 build |

---

## What we rejected, replaced or refused to fake

1. **“Just change the final total.”** Replaced with real delivery-level and phase-distribution calibration; a plausible 185 doesn't excuse impossible dot/single or century patterns.
2. **“Nerf every batter to fix a leaderboard.”** Replaced with separate player ratings, workload, squad membership/availability and whole-season analysis.
3. **One permanent rating for a player across every format and decade.** Replaced with canonical identity plus format/era Performance Profiles.
4. **A match result can be silently rerolled.** Rejected. A limited, audited one-use correction was separated from ordinary play.
5. **Run everything as a simple round robin.** Replaced with domestic stage graphs, group qualification, linked editions and customizable structures; invalid progressions must be rejected.
6. **Handwritten/generic captain “personality” determines outcomes.** Rejected in favour of neutral, rule- and match-state-aware bowling/captaincy, while retaining manager control.
7. **Publish the final score before the first delivery.** Replaced with spoiler-conscious chronological commentary and scorecards derived from the physical delivery record.
8. **Pretend every player hand, bowler type and historical XI is verified.** Replaced with source provenance and explicit unknown/provisional values.
9. **Change the scoring engine to make commentary sound exciting.** Rejected; presentation is downstream of scored events and must preserve seeded results.
10. **Treat all rain as a random abandonment or claim licensed DLS.** Replaced with Standard D/L resource accounting, legal reductions and constrained bowling workload handling; Professional DLS remains out of scope.
11. **Rewrite the game for Windows.** Rejected in favour of preserving the HTML simulator, investigating a host, and ultimately distributing the reliable offline HTML player build first.
12. **Pretend unfinished tests passed.** Several candidates timed out, proved too broad, lacked real Windows storage access or did not match Cricsheet closely enough; the stable fallback and later alpha were clearly separated.

---

## What was present at the first public release?

**CricSim Alpha 1.4 contains:** offline matches, multiple standard formats, built-in/editable teams and squads, batting and bowling profiles, playing XI/captain decisions, conditions and rules, leagues and more complex competitions, tournaments with saved scorecards and records, a Match Centre, commentary, forum exports, qualification analysis, limited player care, and opt-in portable save-folder snapshots.

Its compressed World Pool contains **10,877 player identities, 72,302 performance profiles, 415 team records, 3,295 squad snapshots and 342 competition catalogue entries**. These are **data records, not a guarantee** that every historical competition has a verified roster or one-click full season.

**Important open work:** Full user-origin Windows file/restart and physical PC-to-PC save restoration, very large/long-running universes, the unresolved weak-part-time-bowler and mid-rated-batter balance questions, specialty-format injury coverage, and live administrator installation of custom PlanetCricket BBCode. The native desktop executable is still a separate development track.

**The point of releasing now:** Feature growth has reached a useful threshold. The next advance isn't necessarily another screen or another league. It's watching independent players build seasons, inspect unrealistic outcomes, challenge ratings, move saves and find issues that a single developer and controlled tests haven't exposed.

---


# Part II — The longer development diary

The nine chapters above provide the **chronological overview**. The following is the detailed account of the problems, experiments, corrections, unshipped ideas and design decisions behind them. Here “we” describes the project’s user-directed and AI-assisted iteration, not a claim that every line of inherited or generated code was personally authored unaided.

## Diary 1 — June: the simulator we actually started with

**The original game already worked.** The inspected 1 June 2026 source artifact is titled **CricSim v11.6**, with a filename referring to an **IPL 2026 v3** build. Its header says “Share this file freely.” It has a functional sidebar with **Home, Quick Match, Tournament, Teams & Squads, Archetypes, Match History, All-time Statistics and Documentation**. This is a much more informative starting point than a fictional story about creating a cricket simulator from an empty text file.

A user could choose **T20, ODI, Test, T10, The Hundred or custom overs**, change pitch, weather and toss, select teams and their squads, apply quick conditions presets and simulate. The tournament page advertised **round robin, IPL league with playoffs, knockout, World Cup stages and Swiss**. Those were **already visible** in June; later versions made them safer, more realistic and more configurable rather than inventing them from scratch.

The **player editor and rating guide** already had independent 1–20 batting and bowling skills and different batting and bowling styles. The batting archetypes included **Basher, Dasher, Technician, Anchor, Blocker and Pinch Hitter**. A batter could be more comfortable against pace or spin. The documentation described separate match formats, batting gears and bowling-phase preferences. The basic fantasy premise was already here: choose a cricket world, edit its people, and let the match engine decide the cricket.

The home screen even contained a mini-changelog from **v10.1**. An earlier scoring system had used separate scoring and wicket passes with scalar multipliers. The revised **Pitch × Bowler Type** outcome tables treated a dot, single, two, three, boundary, six and wicket as competing outcomes in one ball-level distribution. Bowler and batter quality shifted those weights. Spinners were preferred on turners, pacers on seam-friendly tracks; run-out ends, late-chase urgency and extras had specific rules. These were inherited/earlier foundations; October calibration refined them.

**But the limits were structural.** The June database used browser local storage for teams, matches, global statistics and tournaments. The implementation wrote only the **last 100 Quick Match records** on save. It was possible to share the HTML file while leaving the actual cricket universe behind in a particular browser profile. The gap between a **portable game** and **portable saves** would become increasingly serious as tournaments and commentary produced larger records.

### The owner's early philosophy was unusually specific

The June 1 conversation described **improving an existing/acquired cricket simulator**. The user wanted real source changes, not general advice; Test, ODI and T20 had to be audited with evidence, not assumed correct. Manager-edited skills and ratings must remain editable even when seasons were underway.

On **2 June**, the owner expressly rejected **automatic ageing, career progression, involuntary regression, retirement and hidden rating changes**. This wasn't opposition to cricket rules governing a batter retiring hurt; that match-law feature arrived much later. It was opposition to the game hijacking a commissioner’s roster over time. The player and league would be a manually curated simulation universe, not an automatic football-manager-style ageing engine.

Another foundational distinction was that **tournament/edition stats are not the same as global career totals**. Later work on canonical identities, tournament families, historical editions and scoped statistical tables comes directly from this requirement.

The June build also explains why a plan to **rewrite the whole game** would have been wasteful: there was already a deep cricket engine and several working modules. The challenge was to preserve that investment while improving what could be tested and trusted.

**Evidence boundary:** the v11.6 file proves what the controls, architecture and documentation contained. It doesn't certify every early format, option or tournament path through a modern browser regression. We do not have the first May prototype or a complete contemporary list of who authored the inherited base.

## Diary 2 — July and August: trying to understand cricket at world scale

Two parallel ideas develop in the available July evidence: **research the actual cricket structure** and **make a growing codebase maintainable**.

### The world-cricket research work was not the same as a roster import

On **19 July**, an archived Volume 1 of a **World Cricket Research Project** covered the ICC and India. Its scope was official bodies, associations, membership, development pathways, domestic and franchise competitions, active versus defunct tournaments and what their structures actually meant. The user wanted one **continuous research document**, developed in phases, rather than scattered summaries or a fabricated database.

This mattered later. A cricket simulator that understands only “ten teams play round robin” cannot accurately represent Ranji Elite/Plate, County divisions, qualification through multiple stages, different cup structures, championship groups, a changing IPL fixture count or a franchise that changes identity. Competition administration has history. Team identities persist even when they leave an edition. A player can be in multiple distinct squads across formats and eras.

That July research should be described as **a documentary precursor** to the World Pool and Competition Atlas, not as proof that every national/domestic tournament was playable in July.

### The source-management problem began early

An archival inventory records a **modular-source package dated 19 July** and a **patched modular package dated 26 August**. That supports a source-organization lineage before October’s GitHub branching. The inventory does not say what exact August bug the latter fixed, so the timeline doesn't invent one.

The design pressure was clear: preserve a **single self-contained HTML file for players**, but make source chapters, changes and tests inspectable for developers. We could not responsibly edit a multi-megabyte code file by pasting arbitrary fragments and hope nothing else changed. Eventually the repository would maintain ordered source chapters, parent-file checksums, byte-for-byte reassembly and seeded match parity.

### A subtle problem was already forming

“World Pool” could mean many different things: all players ever recorded in Cricsheet, contemporary IPL squads, profiles separated by year, verified bowling styles, a catalogue of real competitions, or the actual current team selected by a user. Those are **not interchangeable**. Later source research would uncover player identity ambiguities, incomplete season memberships, generic bowling metadata and unsupported 2026/27 rosters. The desire for breadth and the duty not to invent facts often pulled in opposite directions.

The early months therefore gave CricSim two permanent responsibilities: **respect the cricket domain** and **preserve the user's editable state**. Every major October/November subsystem can be read as a more exact expression of one of those responsibilities.

## Diary 3 — September: why matching 185 runs was not enough

The biggest shift in the project's methodology was moving from “this result seems plausible” to **empirical distributions, same-seed comparisons and whole-season behaviour**.

A simulator may average 185 in T20 while getting there with too many dots and fours, too few singles and twos, too many all-outs or impossibly durable century-makers. A plausible ODI total may mask badly wrong powerplay and death-over patterns. A Test may score the expected runs but never draw because its session clock, declaration decisions and second-new-ball handling are wrong.

### V16 — reconstructing how runs happen

Cricsheet delivery archives provided reference distributions for **dots, singles, twos, boundaries, wickets and extras**. Earlier outcomes were too often a pattern of dot, boundary, dot, wicket. The goal was not to “add a few runs” but to make innings accumulate like actual cricket.

The V16 report describes **100 complete IPL seasons: 7,400 matches, 14,800 innings**. It reported **187.4** average first-innings runs versus **188.5** in the pooled 2023–26 IPL reference. That sounds close—until the same audit showed **11.4% all-outs versus 7.5%**, and **36.2% 200+ scores versus 41.3%**. Matching an average was merely the start of a calibration decision.

### V17/V18 — the difference between a hot team and an immortal player

A 240-run innings can be produced through several batters accelerating. It does not require one mid-rated opener to routinely finish on 150 not out. Orange Cap and Purple Cap tails, hundreds, fifties, 50-to-100 conversion, No. 10 in the batting leaderboard and individual wicket hauls had to be tested separately from team totals.

The V18 audit ran **three independent thirty-season samples**, or **90 IPL seasons / 6,660 matches**. It documented leader distributions, including Orange Cap winners averaging **762.2** and the Purple Cap leader averaging **27.5 wickets**. The upper tail remained sensitive to innings structure and player participation. Overdoing a global batting boost could fix an innings average while making leaders absurd.

### V19–V22 — rebuilding ODI realism on its own terms

**V19** used **1,200 seeded ODIs** across four seeds, tracking 5.771 RPO, century frequency, bowling hauls and innings distributions. That was useful, but its small synthetic team pool was not identical to a real World Cup field.

The **V20/V21** work separated player-quality packs, ODI lineup construction, specialist death bowling, batting-order survival and the difficulty faced by pure bowlers. It found two distinct failures: middle-order batters at Nos. 5–8 sometimes disappeared too quickly, while Nos. 9–11 could score too freely. Their solutions needed to be different. Weak part-time death bowlers were another problem; the engine needed to reserve enough specialist overs, not blindly spread the ball around.

**V22** isolated the **team high-score tail** and the **individual long-innings tail**. Thirty 45-match ODI tournament runs checked 300+, 350+, hundreds, 150+ scores and leader totals against the 2019/2023 World Cup archive. In the final V22 cohort, first-innings 300+ and 350+ frequencies were **46.7% and 21.4%**, compared with reference **48% and 20%**. The model still overproduced fifties and all-outs. Attempts to add generic nervous-forties pressure, indiscriminate 120+ batter protection, a heavy 320+ chase penalty or blanket No. 11 nerfs were **rejected after tests**.

### The Test problem couldn't be solved by applying white-ball logic

Early Test audits measured innings lengths, draw rates, sessions, follow-ons, declarations, 50s/100s/150s/200s, and how frequently the fourth-innings chase actually mattered. There were provisional three-team baselines with unrealistic draw/declaration patterns, so we didn't use them to force ratings. The later red-ball work would differentiate 3-, 4- and 5-day matches and historical eras.

### A new acceptance rule: protect everything you didn't intend to change

Format-specific regression tests appeared across v11.17–v11.22. One recurring dry-game guard contained **630 IPL, 623 T20I and 213 ODI fixtures per version**. A change to ODI death overs shouldn't alter an unrelated T20 match. A commentary change shouldn't introduce one extra RNG draw and cause the scoreboard to diverge. This test discipline was repeated across candidates; a repeated fixture family was **not** fresh independent cricket evidence simply because it was rerun in a new version.

By late September, CricSim had begun to behave like a serious simulation project: **real data for calibration, whole seasons for failure detection, and exact match parity to protect established behaviour**.


---


## Diary 4 — Late September to 8 October: cricket laws, captaincy, identities and domestic seasons

September produced two competing priorities: **fine-tune scoring distributions** and **make the rest of the simulator behave like the sport**. An excellent probability model still fails if the bowling captain ignores workloads, weather produces illegal innings lengths, scorecards miss wickets, or a Test never sees a proper change of session.

### Venues became cricket environments rather than names

The **v11.13 Test venue** effort used regional and venue-specific evidence to assign home-ground pools to the twelve ICC full-member national teams. Missing grounds were added, including Perth Stadium, Galle, SSC Colombo, Sylhet, Chattogram, Bay Oval, Queens Sports Club, Stormont and Sir Vivian Richards Stadium. Venue pitch identity was treated conservatively: an aggregate match average doesn't automatically reveal whether a ground helps finger spin, wrist spin, swing or pace.

A later **v11.20** venue/new-ball bundle extended the context to day/night conditions and the **second new ball** in multi-day cricket. The idea was not simply to add cosmetic controls; the effects needed to apply at appropriate innings times and preserve established match outcomes when those conditions were absent.

This research exposed an unresolved tension that would survive: if an era, a pitch, a venue and a bowler type each change scoring, their effects can multiply into something extreme unless bounded and tested together. Later Phase 2U pitch matrices explicitly corrected cases where a spinning/green ODI pitch scored *more* runs than neutral.

### Rain became a full match-state problem

By the time **v11.28** arrived, the engine already had manual rain/Duckworth/Lewis concepts, but optional **automatic weather and multiple interruptions** added more difficult state transitions. A match can have rain before the first ball, during the first innings, between innings, or during a chase. There are legal-over thresholds, revised targets and bowling quotas that must be repaired when the innings is cut short.

The v11.28 report describes weather probability profiles derived from recent Cricsheet match interruptions: a broad T20 baseline of roughly **5.6%**, ODI **10.2%**, modified by regional weather. Its stress trials included **5,000 clear T20**, **5,000 humid-ground T20**, **3,000 clear ODI** and **3,000 overcast ODI** weather cases, with event chains and no-result shapes tracked. The dry-scoring system was purposely not reopened; 60 ODI, 60 T20 and 12 complete Test seeded matches matched their preceding build with dynamic weather disabled.

An important distinction remained: **Standard Duckworth/Lewis resource tables** were implemented, not the licensed professional DLS calculator used in televised modern international cricket. Automatic weather stayed off by default. This is an example of doing a bounded feature well instead of advertising a rule model we didn't possess.

### Super Overs needed a proper historical record

Early versions could decide a Super Over winner without preserving every delivery of the extra innings. **v11.28** retained its scorecard, commentary, legal and illegal deliveries, bowlers, wickets and partnerships, so that a viewer or publisher could reconstruct why a tie was broken. A 50-seed parity audit showed the same winners and rounds while changing **record retention**, not the resolution itself.

That distinction—**play the event once, store what occurred, generate readable views later**—eventually became the principle behind the Match Centre, session/day reporting and forum publisher.

### Bowling selection stopped being a black box

The **v11.34–v11.36** series dealt with a recurring mismatch: the manager could pick an XI and approve an attack, then the *bulk tournament simulator* might select or rotate bowlers differently from a Quick Match.

The solution was to make the two paths share the same **Auto XI** logic and preserve **Shall Bowl / Can Bowl** intent. Shall Bowl identifies expected primary bowling resources; Can Bowl is a legitimate fallback, not a signal that a batter should suddenly bowl four overs before the specialists. Phase preferences—powerplay, middle, death—matter, as do wicketkeeping, batting depth and bowler workload legality.

In **v11.36**:
- Bulk tournament Auto XI was changed to carry the same bowling intent as the lineup editor.
- Where the underlying squad allowed, the automatic XI aimed for **seven batting-capable options**, rather than blindly fielding too many pure bowlers.
- Knockout, World Cup, Swiss and IPL bulk paths were fixed so **all actual matches**, including playoff games, fed player statistics exactly once.
- Bowling-intent and deployment events could be retained for audit.
- Seeded explicit-lineup tests protected 40 T20, 40 ODI and 12 Test results against unintended engine changes.

The project began treating **MatchCaptain** as a neutral, deterministic actor making cricket decisions within the user's permissions. A proposed subjective captain-charisma rating was rejected. Strong bowlers should be deployed intelligently, but the manager's explicit Shall/Can choice still governs what is legal.

This is an important moment in the game's evolution: it stopped being enough that a XI was mathematically valid. **The captain had to use the right people, at the right phase, for the right reasons.**

### v11.37 solved the "same person, different cricket" problem

Early squads contained player objects. That approach makes it easy to accidentally create one MS Dhoni for India T20, another for India ODI and yet another for Chennai, then merge or separate their career records incorrectly.

**v11.37** formalised three separate concepts:

1. **Player Identity:** A persistent real or fictional person, ideally tied to a stable Cricsheet person ID.
2. **Performance Profile:** How that person played in a format and period—batting and bowling skill, style, empirical fingerprints and era ranges.
3. **Squad Membership:** A reference placing that identity and chosen profile in one team and one squad.

The new **Players** page let commissioners browse, create and edit identities, manage multiple profiles, switch which profile a squad used, and separate accidental same-name collisions. The existing match engine still received a normal materialised player at match time, so this was an **architecture upgrade rather than a simulation rewrite**.

The initial legacy migration test created **198 canonical identities and 265 profiles** from **265 memberships** in the default data with no missing links. A controlled multi-team Dhoni example established that editing one shared T20 profile could update connected memberships without altering an independent ODI profile.

That architecture eventually supported the much larger compressed World Pool. But the existence of a profile from Cricsheet doesn't by itself prove that its batting hand, bowling arm or team contract is fully verified. That problem became a major project in mid-October.

### Domestic cricket was too complicated for a generic IPL generator

The **Phase 2T Domestic** work researched **30 competition templates, 87 stages and 690 fixture definitions**. The issue was not merely adding league names to a dropdown. Real domestic competitions can have:
- Parallel divisions with separate tables.
- Elite/Plate or group stages with different qualification paths.
- Championship or promotion groups.
- Invited teams and academy fixtures that should not be treated as ordinary title-deciding games.
- Different match formats, scoring conditions, and playoff routes.
- Seasons with fixed fixture counts and special cases, such as the West Indies Championship warm-up/final arrangement.

A bug in LOI team selection caused a crash and was fixed; other structural corrections ensured the West Indies warm-up didn't distort champion and statistics accounting. One recurring demand was to **test the actual tournament graph**, not merely verify the JSON template parsed.

**Phase 2U** went back to player and pitch realism after the domestic structures existed. Seven controlled surfaces were compared in **160 T20, 120 ODI and 40 Test innings per surface**—**2,240 first innings** in one full matrix. It also completed season checks including the Metro Bank One-Day Cup (77 games), Sri Lankan three-day FC (63) and multiple BPL seasons.

The BPL exposed a deeper problem. A player who appeared in only six real matches could be present in a fixed simulator squad for twelve or more, collecting an implausible total despite reasonable per-match skills. **We declined to nerf all good batsmen**. The proper long-term solution involves **competition-specific availability/appearance data**, preserving manager authority.

### Tournament "history" needed genuine edition boundaries

The **Phase 3A–3C** work, exemplified by **v11.45 on 8 October**, moved CricSim beyond “finish season, reset everything.” A **tournament family** could own multiple editions without merging their results, entrants and rules into one mutable season.

A new edition could take only selected returning teams from the previous edition, reactivate an archived team, introduce a fresh club, and change a **five-team** league into a **fifteen-team** format. An older edition would retain its actual five entrants and scorecards rather than being retroactively rewritten.

Some rule graphs are resizable; many aren't. A single generic league can inherit rules with a new team list. A graph with explicit qualification slots, parallel divisions or fixed named entrants must instead force a fresh structure choice. The commissioner is not supposed to discover months later that half the participants were silently dropped.

This was also why identities, team status, archived squad snapshots and the backup schema had to mature together. A persistent *world* isn't just a collection of scores—it is a history with boundaries.

### Commentary began to remember the right past

The **v11.59–v11.63** direction introduced limited tournament-aware storytelling. It can refer to **earlier games in the same edition**, then selectively to relevant prior editions: former champions, title defences, rivalry results and genuine player/team records. But unrelated tournaments and Quick Matches are not evidence for an IPL commentary line. Neither is a future fixture whose result hasn't occurred.

This requirement produced an enduring design rule: **freeze factual pre-match context and only narrate what the match knows at that point**. The same principle governed in-innings spoiler avoidance and later mathematical qualification summaries.

### What this phase accomplished

By the start of October, features that once appeared separate—rain, bowling order, player data, domestic competitions, tournament editions, commentary—were revealed to be **one connected state system**. A complete match required the correct squad membership, legal playing XI, conditions, captain decisions, ledger, scorecard, standings and archive. The more CricSim resembled real cricket, the more important its internal consistency became.


## Evidence and dating notes

The main October/November dates are grounded in dated **v11.63–v11.98 validation reports**, **v12 RC/P8–P12 development records**, and the **89 current branch-tip / 68 PR** inventory of the private HFCricSim repository. The early June foundation is recorded in historical conversations and an engineering dossier; the July/August dates come from archival source-file inventory. The September-to-November testing history is separately documented in **[Validation History](./VALIDATION_HISTORY.md)**.

The private branch collection includes experiments, source-only drafts, rejected candidates and release-transport branches; neither a pull request date nor a version label alone proves deployment. Individual October branches often appeared on the same date because earlier work was imported into GitHub—this is **not** proof all those features were invented in one day.

**Further reading:** [Alpha 1.4 download](https://github.com/MaqdoomAli/CricSim-Releases/releases/tag/Simulator) · [Player guide](./README_PLAYTESTERS.md) · [Validation and stress tests](./VALIDATION_HISTORY.md) · [Known issues](./KNOWN_ISSUES.md).

*Compiled 20 November 2026. The document is a feature-evolution history, not a new test run, a claim of sole manual authorship, or an assertion that all experimental branches were merged into Alpha 1.4.*
