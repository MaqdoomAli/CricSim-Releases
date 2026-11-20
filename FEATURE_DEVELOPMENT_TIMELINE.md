# CricSim — From the First Simulator to Alpha 1.4
## A feature-by-feature development history | May–20 November 2026

**Starting point:** An existing offline cricket simulator actively expanded by 1 June 2026, with a dated v11.6 IPL 2026 HTML snapshot.  
**Public milestone:** CricSim v12 Alpha 1.4, the first public PC playtest, released as a self-contained HTML game for desktop Chrome/Edge.  
**This document:** What changed, why we changed it, what was retained or replaced, and which branches remained experiments.

> **Dating note:** The developer reports starting work in **May 2026**, but the exact date, file and feature set of the *very first* prototype have not been recovered. We have an inspected **v11.6 IPL 2026** HTML snapshot archived as 1 June, a modular-source archive dated July, another source snapshot in August, dated September validation reports, and much stronger source/branch records from October onward. Dates below represent the earliest documented milestone or report, **not necessarily the day a feature was first conceived or written**.
>
> **Release note:** A feature appearing in an experimental branch does **not** automatically mean it shipped in Alpha 1.4. Status is explicitly distinguished below. The first public “PC release” is an **offline HTML/JavaScript application opened in a PC browser**, not a certified native Windows EXE.

---


## How to read this history

This is deliberately longer than release notes. There are three ways to use it:

- **The overview (Chapters 1–9):** Chronological summary of how CricSim changed from May through the Alpha 1.4 release.
- **The development diary (Diaries 1–11):** The whole journey in detail—early code, exact bug reports, user decisions, failed fixes, architecture and testing.
- **The appendices:** A dated version/feature ledger, side-by-side evolution of each major system and source/verification caveats.

**Navigation:** [Overview](#the-journey-in-one-picture) · [Detailed diary](#part-ii--the-longer-development-diary) · [Version/feature ledger](#appendix-a--the-release-and-feature-lineage-at-a-glance) · [System evolution](#appendix-b--how-the-same-feature-changed-across-versions) · [Validation history](./VALIDATION_HISTORY.md)

### What the status language means

| Term | Meaning |
| --- | --- |
| **Existing by** | Visible in an inspected old build; not a claim that this project invented it then |
| **Implemented / tested candidate** | Actual code and specific acceptance evidence exist, but no automatic promotion to public release |
| **Experimental / draft / research** | A real design, code or evidence effort with incomplete or deliberately blocked integration |
| **Stable fallback** | Historically identified safe reference (v11.66); not the latest feature-complete build |
| **Public Alpha 1.4** | Actual player-download HTML containing the documented playable features, with open field acceptance |


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



## Diary 5 — 9–13 October: the most intense branching period

The private repository history creates a trap for anyone making a simple timeline: **many branches were created or imported into GitHub on 9–11 October**, but that does not mean all the underlying development happened in those three days. The correct chronology follows the implementation lineage and dated reports, not just the PR creation timestamp. Several branches are experiments, some are archived, others are source-copy or transport branches, and a number never became an installed stable build.

This was also the point at which the project chose **stable v11.66 as its fallback**. We were free to explore aggressively, as long as experiments didn't overwrite the known working HTML or corrupt personal cricket universes.

### Session/day match reports: solve a human storytelling problem with saved events

The user wanted to publish a Test or first-class match **session by session** on a forum without having to stop and resume the simulator. The obvious naïve solution—simulate to lunch, save, publish, then resume—would introduce massive complexity in random number state, batting partnerships, bowler spells, Test time, weather and declarations.

**v11.63** used a better idea. **Simulate the whole match once**, then derive a retrospective report for Day 1 morning, Day 1 full day, Day 2 afternoon or whichever saved interval was requested. Reconstruct a *provisional* batting/bowling/extras/FoW scorecard using only deliveries that had occurred by the period end. Never copy a completed scorecard backward in time. An early session must not mention the future century, declaration or final result.

If an old saved match lacks an adequate delivery ledger or match clock, the game should say the report is unavailable rather than invent its missing past. This was a major shift toward **event-sourced publishing**: a match's saved deliveries become the authoritative source for many views.

### v11.64: even a correction requires rules and a history

Managers sometimes discover a genuine selection error after a match: perhaps a bowler was missing from the designated XI or an eligible bowler was given the wrong permissions. Users needed a recovery path, but an unlimited Undo/Resimulate would allow undesirable “roll until my team wins” behaviour and break downstream qualifications.

**v11.64** created a **single audited correction** for the latest eligible ordinary league fixture. The commissioner had to make a genuine XI or bowling-intent change and provide a reason. The old and new results remained in an audit history. Complex dependent brackets and Test/FC fixtures remained locked until the effects could be safely reconciled.

**v11.69** later fixed a related UX problem: reordering the batting XI could be a genuine correction, but the modal treated the same eleven names in a different order as no change. Up/down batting-order controls and order-sensitive validation made the exceptional correction usable without creating a regular reroll button.

This is also where the **commissioner preflight** and **match-ledger auditor** appeared as independent pure modules. They could inspect a XI or delivery record without needing to run another cricket match. Invalid selections should be explained clearly *only when invalid*, not flood every normal pre-match screen with warnings.

### v11.65: commentary should be vivid without lying

The earlier commentary overhaul aimed to make two consecutive overs feel different because the match situation differed. It could reference a maiden building pressure, the last boundary, the ongoing partnership, an ODI's changing equation or a Test batter surviving to tea.

But the implementation was not allowed to **consume fresh outcome RNG**, alter a bowler's figures, change the run count, invent a fielder or turn “went for four” into a physically false description. Same-seed browser comparisons across T10, Hundred, T20, ODI, FC and Test established that scoring outcomes stayed fixed while narration changed.

The lesson survived through P8: **better writing cannot be purchased by rewriting cricket history**.

### v11.66: a genuine stable checkpoint after serious defects

An invalid custom final with four named entrants had once generated a single two-team fixture and discarded the others without warning. That is a catastrophic bug for a custom league even though the individual match engine is perfectly fine.

The **v11.66** graph validation began rejecting impossible entrants, duplicate transition IDs, invalid qualification slots, repeated ranks and stage completion while a match remained unplayed. It also corrected a subtle statistics problem: a fixture assigned to Division 1/2, Elite or Plate could carry a generic stage tag and therefore **disappear from division-specific player statistics**.

The branch ran seven domestic structures through controlled outcomes and **30 consecutive County Championship editions with 3,780 controlled fixtures**. A separate **998-match, 54-edition, 313,975-legal-ball** real-engine test audited scorecards, bowling, extras and reconstructable player totals. These are different kinds of proof and must stay distinct.

**Stable v11.66** was not the end of development; it was the baseline that made deeper experiments survivable.

### Save experiments: from a giant JSON to recoverable records

Two persistence problems were appearing simultaneously:
- The old browser-origin save data could be lost or inaccessible on another PC.
- Full-universe backups grew dramatically when commentary and detailed historical match events were included.

**v11.67** explored compressed, multipart, lossless universe archives. A 308-match / 31-edition source record collection of roughly 127.81 MiB compressed to roughly 14.01 MiB and decoded with matching records in an in-memory test. A much larger real-engine archive hit a **180-second serialization timeout**, demonstrating that correctness alone isn't enough if exporting an enormous universe freezes.

**v11.68** explored staged extraction from IndexedDB, segmented compressed archives and atomic restore. A synthetic experiment with **54 editions and approximately 504 MB of very repetitive source text** completed an actual browser-process close/reopen with 54/54 SHA-256 record matches; its seven compressed parts were far smaller. That was encouraging durability engineering, **not proof that arbitrary 504 MB real scorecard histories transfer quickly**.

Later RC6–RC9 and Alpha 1.4 work inherited this understanding: check that a transaction committed, read data back, preserve old records until verification, distinguish a valid backup from a merely clickable Download button, and never silently erase the user's only copy.

### Cricket laws: the Hundred and T10 were not miniature T20s

**v11.70** came from a concrete user-supplied match report: Jasprit Bumrah had been credited with **25 legal balls in The Hundred** when the applicable maximum was 20; a T10 bowler exceeded his legal quota; and a wicketkeeper was simultaneously assigned an outfield catch.

Those were **actual laws and scorer integrity defects**. The correction enforced format-specific bowling caps, active wicketkeeper placement, single keeper designation, coherent caught-fielding descriptions and accurately calculated scorecards/FoW. It also fixed a built-in Ravi Bishnoi profile incorrectly using seam rather than leg-spin without overwriting manual edits.

**v11.71** hardened rain-reduced bowling quotas and wicketkeeper exclusions across scripted T20/ODI games. The general rule became: when the attack runs out of a legal bowler, **fail visibly or choose a lawful backup**, not quietly let an illegal spell happen.

### Sixes and Beach Cricket were a separate domain

**v11.72–v11.73** explored Hong Kong Sixes and Beach Cricket, where ordinary eleven-player expectations do not apply. Hong Kong Sixes rules differ by edition. Archived 2024 pre-event announcements did not fully match actual played scorecards; we corrected the **played 2024 and 2025 rulepacks** to their evidenced 6×6-over arrangements and labelled preliminary proposed formats as announcement-only variants.

The Beach Cricket experiment included a small illustrative 2D field, sand-slowed running-ball behaviour, fixed-end/pair and one-handed-catching/boundary-deduction rules. The field sizes and coefficients were explicitly **engineering assumptions**, not surveyed historical measurements or a full physics engine.

These formats gained separate scorekeeping and real playtest interest, but were not given a free pass to ignore rare laws. They later required special handling in forum exports, fixtures and (still pending in Alpha 1.4) the general injury system.

### Flexible mixed-format series changed tournament architecture

A user requirement drove the v11.74 branch: **“Give me two Tests, three ODIs, five T20s, three Hong Kong Sixes and one Beach match in the same series.”** Historical national squads were not meant to be a precondition; the manager could create their own teams.

That required a new fixture concept: each match has **its own format, rules, scoring and innings record**, while sharing a meaningful overall series identity. Test has four possible innings; Sixes has its own dismissals and retirement convention; Hundred has five-ball sets. A record schema cannot flatten all of those into two ordinary T20 innings without losing information.

**v11.75** expanded bilateral series into **3–20-team mixed-format competitions** with groups, rounds, knockouts, changing fixture rules and winner advancement. Points, wins, head-to-head and specified seed policy were kept distinct from unsupported all-format NRR. Matches were not re-simulated to fix inconvenient tournament outcomes.

The most important philosophical requirement came from the user: **CricSim should implement the commissioner's chosen tournament rules, not silently impose its own notion of fairness**. If someone chooses unequal groups and asymmetric qualification, the game should support it if logically valid—or explain exactly why it can't.

### v11.76–v11.78: the custom tournament graph versus a usable editor

The structural v11.76 qualification matrix exercised hundreds of competition configurations and **23,637 controlled/stubbed fixtures**. That proves graph wiring, not 23,637 simulated wickets, overs and scorecards.

**v11.77** added planning for arbitrary 2–20 qualifier slots, bracket byes and downstream knockout fixtures. Yet the visual creator was still too technical: root entrant IDs, group stages and transitions demanded knowledge of the underlying graph model.

The **v11.78 Visual Architect** tried to repair that UX: choose teams inside the wizard, create named groups, redistribute entrants, declare per-group qualification routes, validate and **Apply & Create Tournament**. It used real IDs and guarded impossible three-way duels.

Then its Windows builder failed on **multiple closing body markers** embedded as JavaScript text in the large HTML. The repair changed the source-insertion validator, not the cricket engine. The candidate had source and fake-DOM tests, but actual Windows file-opening acceptance still mattered. This episode captures a repeating project pattern: **a powerful feature is not finished if its installation and creation flow are confusing or broken**.

### v11.79–v11.82: a huge data catalogue exposes a UI problem

The game had hundreds of teams, thousands of players and hundreds of tournament definitions. A hidden source limit displayed only the first 100 teams, 150 competitions or 200 player identities in some screens. A user could have an existing team that was simply **impossible to reach** using a long scrolling interface.

**v11.79** introduced category/country/competition filters, pagination, current national/full-member prioritisation and a unified team finder for World Pool, Teams & Squads, Quick Match and tournament creation. Chromium tests exercised **436 teams across nineteen pages**, including final-page selection. The earlier 42-page UI screenshot report was evidence of the problem, not a desirable browsing experience.

**v11.80–v11.82** investigated UI themes and presentation variants: Floodlights, Heritage Blue, Clear Skies, Broadcast, Pavilion, Studio and a hybrid read-only bridge to actual saved game state. Some were **design candidates**, not final live pages. The owner wanted the application beautified without hiding information or inventing disconnected mockups. This is why later Match Centre work focused on **real scorecards, fixture lists and navigation**, not decorative screenshots.

### v11.83–v11.90: correct the players without inventing facts

The World Pool eventually included **10,877 identities, 72,302 performance profiles, 415 teams, 3,295 squad snapshots and 342 tournament records**. Data scale brought the risk of looking authoritative while silently storing default metadata as fact.

The generic importer had labelled thousands of unverified batting hands as right-handed, and countless bowling profiles as right-arm seam. That wasn't a minor display error: the **bowling family can change match-engine behaviour**, especially pace/spin/pitch matchups. Players such as real spinners could bowl as seamers in the simulation.

**v11.83** introduced explicit **unknown batting hand** and conservatively neutral matchup effects where research was insufficient. The verification work that followed checked bowling arm, spin family, birthdates, keeper roles, aliases and exact Cricsheet identities against more than one independent source.

Some fixes were clear and material: wrongly recorded bowling arms, dual-spin roles, left-arm orthodox rather than off-spin, and removing frontline bowling eligibility from wicketkeepers with no proven actual bowling. Names like **Narine, Markram, Shreyas, Tim David, Ishan Kishan, Rizwan and Pooran** became examples of why one coarse generic role label was inadequate. Some cases, including Surya's bowling family or conflicting player histories, remained unresolved rather than guessed.

A proposal to research **400 players** ran into provenance limits. Not every staged identity had independent corroboration; the large all-or-nothing gate was **not** passed. It was superseded by **100-player B001 batches**, where **100 identity/batting-hand records and 90 bowling roles** were corroborated while ten bowling cases remained open.

The owner explicitly required **individual 2–3-source checks**. A broad unknown-to-right-handed migration was rejected. This data-quality discipline was critical: an accurate cricket match simulator needs a trustworthy person, role and squad before its sophisticated ball engine can mean anything.

**What October 9–13 established:** a stable fallback, an event-derived reporting model, audited commissioner authority, stronger laws, scalable backups, cross-format competitions, a usable creator, and a conservative approach to cricket identity data. It also proved why development branches, source tests and final gameplay releases must be kept separate.



## Diary 6 — 14–16 October: historical years, cricket economics and the v12 turning point

The next major discovery was simple to explain and difficult to repair: **having a 2006-rated player does not make a 2006 cricket match behave like 2006**.

### The difference between player era and match era

A 2006 T20 is not a 2026 T20 with the names changed. The frequency of sixes, pacing of the powerplay, wicket preservation, late-innings risk, run rotation, typical scoring platform and applicable playing rules can differ. But a simulation's **year** must not become a magic switch that silently reassigns player ratings, changes the user's fantasy rules or overwrites saved squads.

The experimental **v11.91** era-aware T20 effort tied the selected year to real delivery-level scoring. In synthetic same-XI tests, 2006, 2015 and 2026 scored differently because the ball engine changed **how events arose**, not because somebody multiplied the final runs after the innings.

**v11.92** deliberately separated the **commissioner's fantasy rules** from the historical year. A manager could run a fictional 2006 edition with modern rule allowances chosen explicitly—without the game declaring the concept invalid for being anachronistic.

**v11.93** extended selected-year handling across T20, ODI, Test and FC; **v11.94** attempted a continuous Cricsheet-derived change-point model rather than three hardcoded eras. One early ODI candidate was **blocked** for missing the absolute scoring/rotation target. **v11.95** confined the adjustments and protected modern-format behaviour using matched seeds.

**v11.96–v11.98** returned to the hardest formats: Tests and first-class cricket. Three-day and four-day fixtures require an entirely different draw/declaration/overs calculus from five-day Tests. Some early historical draw corrections were *too* strong, proving how sensitive outcomes are to batting depth and captain behaviour. A subtle Test captain bug defaulted ordinary match states to **defensive** when no tournament pressure objective was explicitly set. The experiment changed that to a **balanced/neutral** default rather than introducing a subjective captain personality.

Later declaration research checked rainfall forecasts and remaining overs, rejected an overbroad prohibition on declaring while behind, and accepted narrower forecast-aware gates. Source-pinned paired experiments compared hundreds of actual red-ball games per engine rather than trusting a single showcase Test.

An important owner principle survived throughout: **match year is chosen by the commissioner, ratings remain independently editable, and MatchCaptain should act through rules and game state rather than private opinions**.

### Source research was catching up with realism

Different cricket eras also meant different actual XI populations. A 2026 team shouldn't automatically be fitted with its 2008 playing XI because the game has a convenient old World Pool snapshot. Likewise, a limited real historical player fingerprint must not be dressed up as a confirmed batting hand or all-rounder role.

The project began requiring **source provenance and a separation between participation and performance**:
- Identity: Who is this player?
- Performance profile: How does the player generally perform in this era/format?
- Squad membership: Does this team have the player?
- Match selection/availability: Was that player eligible and selected **for this fixture**?
- Conditions and captaincy: How was that ability actually deployed?

This later explained apparently misleading season-leader realism. A player may not be unrealistically skilled per ball; the simulator may simply have let him play far more matches than the real-world reference player.

### v12 integration: the cost of many parallel branches

By mid-October we had accumulated a stable v11.66 base and numerous experiments in commentary, laws, venues, roster research, mixed formats, source chapters and save tools. That created a new category of defect: **two features may work in isolation but fail when assembled in the same full HTML**.

The v12.0 integration uncovered a duplicate cross-script declaration, resulting in only **50 of 51 executable scripts** loading together. That is not a cricket probability bug. It is a consequence of growing a single-file application through layers of patches and wrappers. The complete executable page had to be assembled, opened and tested as one application.

The source work targeted a portable, editable architecture: **51 JavaScript modules, eleven CSS modules and an HTML shell**, reassembled into one offline HTML. A SHA-pinned parent and source manifest helped ensure an edit to one subsystem wasn't secretly changing cricket elsewhere.

The **RC2 mixed-format integration** closed an especially dangerous gap: the ordinary match and tournament screens understood the selected year, but mixed-format series and custom multi-team paths initially did not. They had to pass a persistent year and optional free-hit/Impact Player rules into the actual engine, with per-fixture overrides and frozen played-match rule snapshots. A fantasy historical Test, T10 or Twenty20 fixture should not change after the competition manager edits an unrelated settings panel.

Three linked six-team IPL-style editions completed **102 real matches** in the RC2 test. The longer ten-team experiment at that stage **timed out** and therefore did not earn a pass. The distinction between “smaller complete test passed” and “bigger stress still blocked” was kept explicit.

## Diary 7 — 15–24 October: making the integrated game useful to ordinary commissioners

The central problem was no longer whether the engine *could* play a complicated tournament. It was whether someone could actually **set it up, follow it, find the right fixtures, understand failures and recover their data**.

### Ten-team IPL fixtures exposed a fairness bug

A ten-team tournament can have the correct number of matches and still give one team an extreme home advantage. One development stress run found fixtures as skewed as **13 home versus 1 away** in an otherwise playable fourteen-game league.

The correction did not rewrite past results. It repaired the fixture generator for new editions and tested **100 different seeded schedules** for opponent coverage, fourteen games per team, seven home/seven away and reverse home grounds when appropriate. The corrected 10-team campaign was run again.

The acceptance sequence eventually completed **740 real IPL-style matches across ten editions**, followed by **148 matches across two corrected-build editions**. All seasons included the proper four playoff games. This demonstrated why checking a fixture count alone would have missed the defect.

Later RC3 work generalized fixture fairness to **4–20 teams**, with different repeat-opponent policies depending on field size. But owner-defined custom competitions retained their explicitly chosen structures. The generator could make a conventional IPL-style schedule sensible without overruling a commissioner's deliberately asymmetric competition.

### The tournament hub had too many places to hide information

A competition could be running correctly while the UI made fixtures difficult to find. Upcoming and Results lists displayed cramped cards, truncated names, too few fixtures or only a small slice of past results. A commissioner had to click around to understand what was going on.

The **Tournament Match Centre** work reorganized fixtures and results into a navigable view with **Upcoming / Results / All**, team and stage filters, consistent score/margin alignment, pagination and links to match reports. The earlier blue/orange Match Centre design was polished further without reducing the actual scorecard content.

A reported tournament-creation regression showed the **simulation year** was in the wrong wizard step and a required value could block creation. Fixing that meant adjusting the actual creation form and regression checks, not only making a more attractive mockup.

The **Sim All** interaction created a related problem. The user said it could feel stuck even when it was still working. We explored progress indicators, match and phase counters, resumable/checkpointed batches and periodic save updates so a long season wasn't an opaque wait.

One RC4 Chromium test completed a **153-fixture Vitality Blast** tournament in one browser run, including a button click. Repeated attempts then stalled or timed out. **One success was not enough to certify repeatability.** Performance work on historical edition commentary and cached records followed, but active editions had to remain uncached and correctly mutable.

### Current squads mattered more than exhaustive historic seasons

In October the user explicitly prioritised **present-day squads** over reconstructing every historical season: if a 2026/27 squad was difficult or speculative, use the most recent verified preceding season and label it correctly.

The **Competition Atlas / Current Squads** direction responded to an awkward World Pool fact: the catalogue could contain thousands of identities and season snapshots without ensuring that a current IPL, County Championship, Ranji Trophy or Vijay Hazare roster was genuinely reachable and complete.

The work included:
- Team/competition/country filters and current-squad-first browsing.
- Exact links between World Pool team records and available squad snapshots.
- England's County Championship team references and season-linked fallbacks.
- Current Indian domestic evidence and clearly marked missing names or partial squads.
- Hundred and BBL franchise, identity and fixture consistency.
- Protection against duplicate squad members and silent replacement of user-edited teams.
- Refusal to label source-only 2026/27 data as authoritative active contracts.

There were source-backed examples where a real 17-player official squad resolved to **only fifteen exact known identities**. The correct behaviour was to offer a labelled fifteen-player subset and record the two missing names, not fabricate their profiles.

This also explained why the user kept requesting that we inspect **every page**: a correct World Pool hidden behind a truncated 100-item list, a broken bowling-stat table or an obscure tournament hub is still a broken player experience.

### PlanetCricket sharing became its own product surface

The owner wasn't just interested in seeing cricket privately. CricSim should allow people to **run a forum league and post it convincingly**: the complete match, every delivery, an ordered story, scoreboard, squads, fixture lists, standings, player stats and tournament identity.

The **normal BBCode** path was deliberately separated from the more ambitious custom-styled one. Normal XenForo markup should work **without waiting for a site administrator**. A longer commentary transcript can be split across sensible forum post lengths while retaining sequence; a result should come after the proper match build-up, not in the header.

The richer **28 custom PlanetCricket tags** and LESS styles were a separate installation package for the forum administrator. A development branch experimented with forum-theme tokens rather than hard-coded colors. Later user-customizable **HEX accents and dropdown colors** were added to the player exporter. These publisher controls should not change ball results or saved match facts.

A specific forum-platform change also affected table formatting: a user reported that **[TABLE=collapse]** needed to become **[TABLE]** in raw BBCode to work under the upgraded PlanetCricket. This was a useful reminder that site markup compatibility lives outside the cricket engine and can change after release.

### DRS was more than a popup after LBW

Decision Review System work began with source evidence on **actual recorded player reviews** in Cricsheet and ICC playing conditions, not simply a random percentage of LBW decisions being overturned.

The prototype distinguished player reviews and umpire referrals, success/retention, batting versus bowling-side belief, appeal eligibility, match format and year applicability. A useful DRS feature must do several things together:
- Decide an appeal at the **correct delivery**.
- Charge an unsuccessful team review only under the selected rules.
- Recognize eligible **LBW, caught behind and bat-pad** review contexts.
- Preserve the scorer's actual dismissal and bowling credit.
- Include the review naturally in chronological commentary and at the right place in post-match scorecards.
- Carry Test review resources through innings according to the selected rule settings.
- Support **Off**, broad review and selected review-type modes without changing non-DRS matches.

Early DRS branches were source/rules prototypes, **not** a finished game integration. RC8 and its later RC8.8 package moved the feature toward a real full-HTML Match Centre and tournament/Quick Match experience. The browser/playtest evidence improved, but exact advanced physics, review frequencies and save/reopen behaviour remained active testing topics.

No full commercial ball-tracking simulation was added; DRS used game state, simulated review evidence and playing-rule logic. Claiming “Hawk-Eye physics” would have been false.

### RC6 → RC7 → RC8.8: packaging and recoverability mattered

Public downloads were sometimes larger than the tool upload path could handle. We had **RC8.8 as a roughly 8.04 MB HTML file** and a **native RC9.0 Windows executable around 14.23 MB** in earlier candidate work. Attempts to move large binaries through ChatGPT/GitHub were sometimes interrupted. Branches containing split HTML payloads exist for this reason—**they are build transport, not new game features**.

**RC6/RC7** focused on whether browser persistence failed safely: a transaction must complete before cached backup data or legacy keys are discarded. A no-op or failed IndexedDB write isn't a successful save because the function returned. **RC7.1** consolidated several storage guards and targeted regressions.

**RC8.8** combined DRS, Match Centre, fixes for tournament completion, restoration safeguards and Sim All checkpoints in an integrated standalone HTML candidate. It was significant because it joined many previously separate experiments into a game people could actually play.

**The lesson of mid-October:** building a cricket engine and building a distributable *simulator product* are different projects. Reliable controls, correct player data, recoverable saves, non-spoiling reports and navigable seasons are all part of gameplay.



## Diary 8 — 23 October to early November: statistics, commentary and "does this feel like a game?"

At this stage the basic feature checklist looked impressive. Yet the user's questions became more demanding: **Is the bowling stats page wrong? What about the other pages? Why does Sim All feel frozen? Does the commentary reveal the winner too early? Does an interesting event read like something actually happening? Are we making cricket better or just accumulating menus?**

These were not side issues. They were signals that the simulator was transitioning from a personal engineering tool into something strangers would try to understand and enjoy.

### The 13-page content audit and the limits of visual polish

The owner asked to list the simulator's pages, inspect them for similar problems to a broken bowling-stat view and revisit what to **add, remove or modify** in each. That is different from a color-theme exercise. It asks whether a screen has the right information, whether records reconcile, and whether the user can navigate from a player to his squad, performance, match and tournament context.

The audit work went through the existing home, setup, player/team, statistical, history and competition surfaces and progressively moved toward a unified **Match Centre and Tournament Hub** style. Earlier theme experiments were useful, but an attractive panel that hides the fixtures or removes a key stat is a regression.

The Match Centre's **blue/orange layout and team accents** were only the visible layer. It also needed meaningful score alignment, chronological deliveries, batting/bowling cards, full commentary and links back into a tournament. Several hover/navigation regressions were caught as the designs evolved.

### P8: commentary had to become a record of play

A complaint about the existing commentary was precise: the line should visibly contain the **delivery number, bowler, batter, outcome and description**. A sample was “0.1 Jasprit Bumrah to Travis Head — 4 — Head sliced it away on the first ball.” The owner emphasized that this was **an example, not a demand to compress every delivery into one sentence**.

P8.5 onward separated **what happened** from **how it is described**. A zero, single, boundary, wide, no-ball, bye, appeal, wicket and review should be presented with enough information to follow the match. A saved score should not be concealed behind a generic phrase or revealed prematurely by a result card.

The system needed to handle:
- Ball coordinates and outcomes across T20, ODI, T10 and Hundred (whose five-ball sets require their own notation).
- Test/FC scoreboards and commentary at session or day intervals without future knowledge.
- Correct run-out end/credited fielder and dismissal attribution.
- Wickets, partnerships, bowling changes, target equations and phase-specific language.
- Full delivery/event data in match reports; no truncated account presented as a “complete” replay.
- DRS reviews at the delivery that triggered the appeal, not as an afterthought below the final result.

**P8.6** reported **25 Chromium-completed fixtures and 7,658 delivery events**, including review linkage. **P8.7** used **26 matches/8,660 deliveries**, with checks for targeted run-out descriptions and reduced repeated commentary. **P8.8** added structured fact tagging and UI zone corrections, reporting **26 matches/8,932 deliveries** and preserved same-seed score tuples. **P8.9** still reported repeated lines, despite improving them. The exact match suites share seeded cases across iterations; they are not four completely independent batches of 25 brand-new cricket conditions.

The project's answer to “do we need a physics system?” was nuanced: **not necessarily a full 3D cricket physics engine** to produce decent text. But a narrator shouldn't claim a ball was driven through cover if the match record contains no reliable shot/placement evidence. P8's structured cues and conservative wording were a compromise between richer writing and factual integrity. Some attractive generated descriptions were rejected or made conditional because they implied physical facts the engine had not actually simulated.

This is one reason we treated commentary as more than cosmetic: inconsistent narration destroys confidence in the scorecard, even when the underlying runs are mathematically correct.

### "No spoilers" became a cross-feature rule

The issue appeared in several places: a report with the winner in the first heading, a Match Centre opening directly on a result, a DRS decision described before the ball, an innings story quoting a final century that hadn't happened, and a tournament note referring to fixtures not yet played.

The fix was **chronological information boundaries**. For a completed match, all events may already be stored; that doesn't mean the viewer should be shown everything at the first instant. The report and interface decide which recorded facts are visible at which stage, without re-simulating the cricket.

The same idea later supported qualification analysis: a mathematical statement that a team *could* qualify is valid; pretending to know a future result or NRR swing is not.

### P9.3: qualification should be mathematical, not a guess

By **2 November**, the Tournament Hub's **Qualification Analyst** could enumerate all possible win/loss/tie-or-no-result points outcomes for suitably small, fully known group schedules. A four-team example began with **729** remaining outcome combinations; after two matches, **81** combinations remained.

It could identify a team that was already secure on points, a team that could still make it, one dependent on tiebreakers, or a team mathematically eliminated from a strict-points path. It also inspected combined championship/divisional tables without incorrectly describing provisional stage results as confirmed progression.

The major safety boundary was the same as elsewhere: **do not invent NRR, future innings scores or qualification probabilities**. The tool could explain a route but not pretend to predict cricket.

The owner reported that P9.3 **worked** and asked to beautify it. That acceptance gave the feature a useful baseline. The natural next question was not “can we calculate even more combinations?” but **“which upcoming fixtures actually matter to my team?”**

### P9.4: turn the analyst into a decision-making aid

The **Decision Centre** was proposed and then developed as a read-only layer above P9.3. Given a selected team, it could highlight fixtures whose result materially affects qualification and describe combinations of wins, defeats or tied/no-result outcomes that leave points routes possible.

It had to respect the P9.3 preflight. If the schedule, stage graph, bonus points or unknown entrants make exhaustive outcomes unsupported, the Decision Centre should **show a limitation**, not invent a confident probability.

This is a genuine gameplay upgrade. A table tells you who has 12 points. A good qualification interface tells you **why a particular upcoming match is important** and what conditional route remains to a final—without playing the match for you.

### The tournament was becoming a living competition

At the same time, the existing edition history and commentary memory meant fixtures could carry background: a defending champion, a recurring rivalry, a player with records in the same tournament family. These must be recorded past facts, never story-generator invention.

The combined result was a shift from **a result-generation program** to **a commissioner-led cricket world**:
- It has a past that is preserved.
- It has a present the player can navigate.
- It has a future fixture schedule the analyst can reason about.
- It doesn't know those future match results before the engine plays them.

That was the real significance of P8/P9, beyond any one panel.

## Diary 9 — Late October to 14 November: the desktop experiment and the road to a trustworthy alpha

### RC9: the native-desktop question wasn't an excuse to start over

Moving to Windows was attractive: a native app could have a more predictable save location, SQLite history, checkpoints and potentially better file-management ergonomics. But rebuilding the match engine as a new C# or Go game would risk discarding months of cricket calibration and parser compatibility.

The owner's direction was explicit: **move the existing HTML/JavaScript gameplay intact**. The RC9 desktop experiment wrapped the familiar UI in a **Go-based local host** with a SQLite persistence layer, snapshot/checkpoint endpoints and integrity guards. There were Windows candidate EXEs; they were not a replacement for stable v11.66 or proof of finished user-universe migration.

The **one-step migration** goal sounded simple: open the desktop game, choose the old universe, and continue with all teams, players, history, tournaments, stats and settings. But the existing HTML's browser-profile storage cannot be read automatically by an arbitrary native app. This required a **full-universe JSON export from the original browser**, schema preflight, explicit user permission and readback.

**RC9.1** made that constraint honest. The guided transfer validated collection types, identity uniqueness, hashes when present and SQLite health before allowing the import. A full replacement required explicit consent and a safety path, especially for legacy backups without hashes. The native pilot also needed to guard against accidental resets, interrupted writes and sidecar/checkpoint corruption.

The repository documents numerous local acceptance checks for source and byte-level rollback, but also repeated **NO-GO** decisions: the user's genuine old world had not yet been proven through full Windows import, process exit and cold restart, and very large live saves had not completed an exhaustive field acceptance.

Those limits directly influenced the final distribution choice. **The first public PC release would be the established offline HTML, not a prematurely certified native EXE.**

### P10: the engine realism investigation returned to player ratings

After major UI and competition improvements, the owner asked to compare all formats with **Cricsheet**, not just trust our T20 headline scoring averages. The P10 work began tracing the route from an imported player profile to the actual selected XI and then to the ball-level batting/bowling skills.

**P10.1** found concrete rating/import wiring issues:
- Some older CSV attributes stored strings where format-rating objects were expected.
- A constructor argument position could shift a player's format-specific label into the wrong field.
- Current pack profile snapshots could mis-wire the intended format override.
- An apparent batting or bowling imbalance might therefore originate from **which rating or profile was selected**, not from the physical ball probabilities.

The later P10 stages examined modern national squads, calibrated current-format cohorts and reviewed the Hundred against real playing-XI evidence. **P10.4** parsed around **23,133 Cricsheet JSON files / 50,954 non-super-over innings**, separating formats, domestic versus international, sex, era and innings phase. Those are **reference records**, not 23,133 simulated CricSim matches.

The **P10.5 national-player acceptance** used real game match functions across T20, ODI, Hundred, Test and first-class. **P10.7** separately compared approximately **408 source-backed Hundred simulations per candidate engine** with real 2026 XI compositions and holdout matches. The central lesson was not “we found one multiplier.” It was **data provenance, appearance opportunity, bowling allocation, player skill and team context** must be analysed together.

At one point the owner argued that the culprit was **ratings rather than the engine**. That was a useful hypothesis, not a reason to ignore the engine. The correct process was to trace the rating pipeline, build matched comparisons and preserve otherwise realistic scoring paths.

### Historical-era research challenged easy assumptions

The P10 and v11.9x streams also pushed back against simplistic ideas such as “every decade has a steady scoring increase.” Actual scoring history is not monotonic. A 2026 men's Hundred match, a 2006 T20 international and a 2015 ODI are distinct contexts; changes to domestic roster quality and selection can be as important as era-wide scoring curves.

We preferred **measured cohorts, labelled incomplete data and no blanket auto-ratings** over artificial uniform historical realism. A manager could still stage fantasy matchups without pretending they were statistically exact reconstructions of the past.

### P11: define what a public release has to prove

As Alpha approached, the owner asked to complete outstanding work quickly enough for playtesters while keeping the game intact. A long feature checklist was no longer an acceptable shipping gate.

The **P11 acceptance matrix** required actual browser startup, navigation, Quick Matches in all main formats, real saved scorecards and statistics, tournament completion, DRS and BBCode outputs, backup/import safety and native restoration where relevant.

Local scripts passed dozens of targeted checks, but they were **not passed off as a full 8 MB application browser acceptance** when browser navigation or the source artifact wasn't available in the test environment. The project learned to label tests precisely: **unit test, source integration, browser smoke, full real-engine run, native process restore, or public field acceptance**.

This was also when the user insisted that **GitHub Actions should be used for uploads/publication, not repeated expensive hosted tests**. Development reports distinguish local Chromium/Node tests from GitHub-hosted runs. A green workflow badge is not a substitute for opening a real old save in the real game.

### P12: injury and retirement laws exposed unusual corner cases

On **13–14 November**, the owner spotted a major missing category: batters can retire voluntarily or due to injury, and an injured bowler might leave the field or be unable to finish an over. The cricket laws depend on whether the batter retired **hurt** or **out**, whether they are fit to return, when a vacancy arises, and which ICC/MCC/domestic playing conditions govern the match.

The design discussion deliberately ruled out a dramatic injury-management minigame. The user wanted **rare, plausible incidents**, not an injury popup every match. There was no desire to have the game randomly weaken an entire career, and no requirement for the manager to nominate a named reserve fielder before every game.

The toughest edge case was **a playing squad containing exactly eleven members**. If a bowler in such a squad becomes unavailable, you can't conjure a twelfth teammate and then pretend the fielding replacement can also bowl. The accepted rule was:
- **Only eleven total squad members:** no spontaneous bowler injury that would create an unresolvable bowling shortage.
- **Batting injuries still possible:** a batter may retire hurt and later return when legally permitted.
- **With genuine eligible reserves:** choose a substitute fielder, preferentially a good fielder, **from the actual squad**.
- **No imaginary bowling rights for the 12th fielder:** a member of the nominated XI must take any remaining over/load, within the format's legal quota.
- **Retired out:** a dismissal without a credited bowler wicket, and no ordinary same-innings return.
- **Retired hurt:** not a wicket; possible return depends on fitness, vacancy and governing rule profile.

Test and first-class return/carryover behaviour added further subtleties. One P12E fix prevented a batter who had *just retired hurt* from instantly replacing themselves in the vacancy caused by that same retirement. Another made automatic risk roll once per **legal ball counter**, not repeatedly on wides/no-balls.

An early **P12D** prototype passed isolated rules/controller checks but was **not** a complete public integration. **P12E Alpha 1.1** wired the medical panel into standard Quick Matches. It still wasn't correct to say all tournaments had P12 just because Quick Match worked. That next step became Alpha 1.3.

### What had actually changed by mid-November

CricSim had acquired far more than new scoring tricks. It now had a real choice to make about release discipline:
- Wait for a perfectly calibrated, native, all-format and all-save-path game—possibly indefinitely.
- Or publish the most coherent **offline HTML alpha**, with the core match and tournament engine usable, transparent known limitations, sensible backups and genuine external playtest feedback.

The owner chose the second approach, without abandoning calibration or data integrity. That is why **Alpha** is a meaningful word here rather than an apologetic marketing label.



## Diary 10 — 18–20 November: from months of private work to a public playtest

The public release didn't arrive as a sudden version bump. It was the result of deciding what a stranger needed on their first day with the simulator: a working game, an understandable README, a believable first match, a way to run a league, a way to save that league, and a way to report what's wrong.

### Alpha 1.1 — the first playable injury integration

The early **P12E injury-stabilised** candidate supplied a full standalone HTML with the match-day medical controls wired into **Quick Match**. This included manual/scripted retirement and injury scenarios, low-frequency automatic events, ICC/MCC/BCCI conditions, eligible return, safe fielder replacement and rules for injured bowlers.

The first branch with P12E was superseded because it did **not** have the final Test/FC care-carryover integration. The later Alpha 1.1 package consolidated those corrections. It was not yet a complete claim that all tournament modes inherited injuries.

One of the hardest lessons of this release period was **feature reachability**. A controller can pass twenty or thirty tests and still not matter to a person who only plays tournaments, because the tournament scheduler never calls it. A feature is integrated only when the real user pathways deliver it.

### Alpha 1.2 — make a league attractive enough to publish

The publisher became a much larger request than “export result as text.” The user wanted every part of a league thread to share a coherent design: **match reports, squad lists, statistics, tables, fixtures and tournament summaries**.

For squads specifically, the design drew on actual PlanetCricket league posts: group **openers, top order, middle order, finishers, pacers and spinners** in readable, recognizable blocks rather than dumping a plain comma-separated list. The export should retain useful team/player information and add relevant **team stats beneath the squad** rather than overwhelming readers with decorative markup.

The owner then asked for an accessible customization step: either **pick a colour from a dropdown or enter a HEX code**, then use that palette consistently when posting or exporting. The same simple choice should apply across the available forum output formats. This was less about making a screenshot pretty and more about giving different forum commissioners a consistent **club/league identity**.

The more polished HTML-like forum presentation relied on **custom PlanetCricket BBCode** that a site administrator had to install. The site had changed its XenForo table handling, and a community fix to replace old collapsing TABLE tags was discussed. Neither custom tags nor a forum-side styling rule could be guaranteed working before an administrator actually deployed them. Therefore the **ordinary BBCode fallback** was essential from day one.

A separate **admin installation ZIP** existed because forum administrators need definitions/LESS/examples; ordinary playtesters do **not** need those files. Later release packaging accidentally mixed the two, and this was corrected before the public user-facing distribution was finalized.

### Alpha 1.3 — tournament injuries and the actual IPL stress

The user rejected the idea that player care should exist only in Quick Match: **it belongs in all normal playing modes**. Alpha 1.3 integrated the care settings through standard tournament and match pathways for **T20, ODI, T10, The Hundred, Test and FC**, with control in creation/fixture settings as appropriate. Existing teams with only eleven players still suppress spontaneous bowling injuries, without disabling batting injury.

The acceptance included actual match runs and an important eleven-player edge-case regression: the initial test itself had overlooked that only one side's squad was set to eleven. Correcting the test exposed why **test setup is part of correctness**.

An actual game-engine run then completed **25 IPL seasons, 1,850 matches including 100 playoffs**. It recorded a small number of retire-hurt and injured-bowler events and checked whether results, scorecards and player stats survived. The 25-season output also highlighted why the project was ready for external examination but not perfect: some **12/20 batters** had extremely productive seasons, and **9–10/20 part-time bowlers** received unusually heavy workloads.

The user did not want artificial “balance” achieved by nerfing them without diagnosis. The next step was to see whether the issue was the **base skill**, match opportunity, captaincy phase logic, playing XI or unusual but valid variance.

**Honest boundary:** the specialty **Hong Kong Sixes and Beach Cricket** engines use their own six-player laws and did **not** receive the same full P12E injury integration in Alpha 1.3. Saying “all modes” without that caveat would be inaccurate.

### Alpha 1.4 — portability finally means the saved game as well as the HTML

The user's final pre-release feature request addressed one of the oldest architectural weaknesses, already visible in the June localStorage database:

> If someone restarts, changes a browser profile or moves to another PC, where is the point they can recover the **whole universe**, including the matches?

The **Portable Save Folder** was built as an **opt-in** layer rather than replacing the established IndexedDB/browser save path. The user chooses a directory, and the game can write a complete, verified universe snapshot, retaining up to **twelve rolling recovery snapshots**. The save is written, checked and only then announced as the latest verified snapshot. Restoring from a directory creates a safety backup before overwriting the current browser universe.

The intended migration flow is deliberately simple:
1. Save and verify.
2. Copy the **entire folder**, not just an HTML file.
3. Open the game on another PC, choose that folder and restore.
4. Keep the original machine and JSON backup until the restored universe has been checked.

A focused test suite covered checksum tampering, snapshot retention, external edits from a second simulated PC, simulated browser-profile migration, and a **five-fixture IPL integration snapshot** that retained five results and **120 player-stat records**. The actual Windows folder-picker permission, physical PC-to-PC move and cold browser restart remain field tests. Chrome/Edge File System Access permissions may need renewed consent. A **Universe JSON export/import** is retained as the fallback.

This wasn't a glamour feature, but it completed an important principle: **a cricket world is worth preserving**. A 200-match league isn't disposable just because someone updated Chrome, moved a file or bought a new PC.

### The PlanetCricket upload failure changed the distribution plan

The first public announcement had an appropriately modest tone: after working since May, the owner was finally comfortable sharing the game and ready to be challenged about its shortcomings. The aim was not to declare the simulator finished. It was to get genuine users producing seasons, reports and odd results we hadn't foreseen.

Then PlanetCricket refused the approximately **5 MB ZIP**, with a server processing error. It wasn't simply a missing HTML extension or a universally verified 5 MB site limit. A public download needed to live somewhere more reliable than one forum attachment-processing path.

The solution was a separate public GitHub **CricSim-Releases** repository, leaving **HFCricSim** development history private. The player ZIP was uploaded as a real **GitHub Release asset**, with a checksum, README, instructions and known limitations. An accidentally included PlanetCricket admin installer was removed from the public upload plan.

The release page itself was initially too thin: it told readers about calibration and a ZIP without showing what the game actually did. We inspected the integrated HTML, wrote about its **actual player profiles, custom tournaments, commentary, scorecards, records, DRS, rules and portable backups**, and changed the release title and designation to a proper **Alpha 1.4 pre-release**.

The first public downloadable PC release is therefore an **offline HTML game intended for PC browsers**, not a pre-certified native Windows installer. No user account or live online server is needed to play ordinary cricket.

### Why it was finally worth sharing

By 20 November, another month of features could have been added. But feature scope had reached a useful stopping point. The bottleneck was no longer "can we add a tournament?" It was **whether actual people find the simulator credible, approachable, reliable and interesting over many matches**.

Public playtesters can now challenge:
- Does the batting/bowling distribution feel credible across formats and eras?
- Is a player under- or over-rated, or simply appearing in too many fixtures?
- Does MatchCaptain use the best bowling resources fairly?
- Can I create the tournament I actually want?
- Do the standings, division statistics, play-offs and scorecards agree?
- Does the result stay hidden until the viewer chooses to see it?
- Can I publish a readable forum league using ordinary BBCode?
- Can I close, reopen, back up and move a long-running universe without losing it?

That is the significance of **Alpha 1.4**. It is not the claim that the simulator has no more shortcomings. It is the first point where the whole experience felt useful enough to let other cricket fans expose them.

## Diary 11 — What the journey changed in our development practice

Some of CricSim's most important developments were **rules for making decisions**, not buttons inside the game.

### From broad requests to source-grounded changes

“Make it realistic” became a demand for **named real-data cohorts, separate formats, distributions and correctly matched player opportunities**. “Improve commentary” became **delivery ledger authority, no extra scoring RNG and no spoilers**. “Build a tournament editor” became **real team IDs, explicit qualification edges, fail-closed invalid graphs and editability without overwriting past editions**.

These translations were hard work. Many suggestions would have been quick to demonstrate but dangerous to ship. A single global run-scoring multiplier, automatic role assignment or permissive migration might look successful in a smoke test while breaking the long-running universe.

### From demos to acceptance gates

One isolated Python/Node unit test is not a complete game. Neither is one successful Chromium screenshot. A feature with a realistic-looking scorecard might have zero accurate player identities. A correct tournament graph might still be too difficult to create. A ZIP might pass CRC checks while its UI fails in a real browser.

The project gradually required **separate evidence** for:
- Source correctness and repeatable full-HTML assembly.
- Exact RNG/outcome parity for presentation-only changes.
- Actual simulated matches and ball-ledger invariants.
- Full tournament editions and stage transitions.
- Statistical comparisons with suitably matched real cricket.
- UI navigation and complete export/report presentation.
- Browser storage and true transaction commit/readback.
- Native process restart or actual copied-folder restoration.
- Public forum rendering under the administrator's installed configuration.

Different reports passed different categories, and the difficult failures were retained: timeouts, wrong fixture parity, unproven historical bowling actions, incomplete squad availability and save-transfer limits.

### From adding everything to choosing boundaries

We repeatedly decided **not** to add a mechanic merely because it sounded impressive:
- No compulsory player ageing or forced career progression.
- No unlimited rerolling of completed matches.
- No subjectively rated captain magic.
- No fabricated bowling arms or false 2026/27 squads.
- No invented all-format NRR, future scores or qualification chances.
- No claim to proprietary Professional DLS.
- No claim that generated commentary has full physical shot tracking.
- No fictitious twelfth player for a squad of eleven.
- No breaking the match engine to obtain a prettier report.
- No claiming an experimental Windows EXE was fully migrated because a unit test passed.

These constraints are part of what makes CricSim recognizable. It gives a commissioner tools and autonomy, while the engine enforces cricket laws and recorded facts.

### Source version versus player release

Much of the private repository contains **experiments, research queues, generated source patches, transport/packaging branches and partial acceptance**. An experiment might be *real and valuable* while still being unsafe to merge.

The public game is a narrower claim: **the assembled offline Alpha 1.4 HTML with its documented capabilities and caveats**. The relationship between old version labels, branch dates and integrated features must always be explained rather than assumed. That is why this history is accompanied by a version-by-version [validation archive](./VALIDATION_HISTORY.md).

### What is still unfinished—and what is not urgent

**Open field acceptance** includes a genuine long-world close/reopen and PC-to-PC folder restore, more browsers and storage quotas, live PlanetCricket administration of custom BBCode, specialty-format injury integration, and a careful review of part-time-bowling/season-leader outliers.

**Not an immediate priority** is rebuilding the entire ball engine, forcing an automatic career economy or adding an unverified physics system simply to grow the feature list. The first public alpha should tell us which remaining problems are important to actual players.

**The development story is therefore a shift in what “done” means:** we began with an inherited working simulator and ambitions for more realistic cricket. We built detailed game systems, had to learn why they conflicted, designed safer data/competition models, stress-tested them, and eventually chose an honest public alpha over endless private feature accumulation.



## Appendix A — The release and feature lineage at a glance

This register is deliberately **not a table of 89 separate product releases**. Many current GitHub branches are drafts, source patches, research archives, duplicated source tips or delivery-only branches. The register follows the *feature lineage*: what the milestone meant, and whether it was part of a stable release, an experimental branch or a later integrated alpha. Dates marked **c.** are approximate and based on archive/report dating.

| Approximate period | Milestone | Feature change or key decision | Historical maturity |
| --- | --- | --- | --- |
| May 2026 | Project begins | Work on improving an existing cricket simulator; earliest prototype not recovered | Owner-reported origin |
| **1 Jun** | **v11.6 IPL2026 v3** | Offline playable interface, Quick Match, six main format/custom settings, teams/squads, archetypes, history/stats, league/IPL/knockout/World Cup/Swiss | Source artifact inspected; earliest direct feature baseline |
| **1–2 Jun** | User scope decisions | Manual player/skill editing; no forced ageing/progression/retirement; separate tournament vs career stats | Explicit owner requirements |
| 19 Jul | World Cricket Research v0.1 | ICC/India governance and domestic competition mapping | Research, not playable catalogue |
| 19 Jul | Modular source ZIP | Separate editable-source direction while retaining portable HTML | Archived source package |
| 26 Aug | Patched modular source | Revised development/source archive; specific patch details not proven | Archival evidence only |
| Sep | V16 | Better dot/single/boundary distribution, 100 IPL seasons | Calibrated historical candidate; residual errors |
| Sep | V17/V18 | Batting and bowling leader-tail tuning, 90-season IPL study | Calibrated candidate, not a permanent guarantee |
| Sep | V19 | ODI scoring phases, hundreds and wicket hauls, 1,200 ODIs | Research and engine candidate |
| Sep | V20/V21 | ODI team pack, player roles, middle-order survival, lower-order scoring, death bowling, CSV fidelity | Calibrated candidate |
| Sep | V22 | ODI 300/350+ tail and 150+ individual scores, 30 tournaments | Historically “locked” ODI pass with known residuals |
| **29 Sep** | v11.13 | Test venue/home-ground pools, verified surface and regional weather identities | Separate venue development |
| Late Sep | v11.17–v11.22 | ODI phase texture, Test second-new-ball/venue context, rain/D/L and protected same-seed regressions | Format-specific iterations |
| Early Oct | v11.24–v11.27 | PlanetCricket match posting, BBCode and styled report development | Forum presentation evolution |
| **2 Oct** | v11.28 | Optional automatic rain, multiple interruptions and full Super Over delivery records | Tested engine extension |
| Early Oct | v11.34–v11.36 | Phase-aware bowling, Shall/Can Bowl, Auto XI parity and playoff stat fixes | Integrated cricket/tournament corrections |
| **2 Oct** | v11.37 | Canonical Player Identity, Performance Profile and Squad Membership | Major data model milestone |
| c. 7–8 Oct | Phase 2T/2U | Domestic graph registry; profile and pitch calibration; complete example seasons | Developed with explicit unresolved BPL availability |
| **8 Oct** | v11.45 Phase 3C | Persistent multi-edition tournament families and safe team-count restructuring | Model/validation milestone |
| c. 8–9 Oct | v11.59–v11.62 | Bounded tournament-memory commentary and pre-match historical context | Iterative narrative design |
| **9 Oct** | v11.63 | Test/FC session and day exports from saved delivery ledger | Browser-tested standalone |
| **9 Oct** | v11.64 | Exceptional one-use commissioner correction; editable domestic templates; shared venue surfaces | Browser-tested incremental release |
| **9 Oct** | v11.65 | Deterministic richer broadcast commentary, score/phase/session pressure | Browser-tested presentation update |
| **9 Oct** | **v11.66** | Tournament graph fail-closed integrity, division statistics and 30-edition structural stress | **Documented historical stable fallback** |
| **9 Oct** | v11.67–v11.68 | Compressed multipart backups, IndexedDB staged restore, browser process durability | Experimental storage beta; not Alpha 1.4 certification |
| **9 Oct** | v11.69–v11.71 | Pure record/preflight checks, batting-order corrections, Hundred/T10 workloads, keeper/rain laws | Development candidates with focused tests |
| **9 Oct** | v11.72–v11.73 | Hong Kong Sixes/Beach Cricket historical rules and dedicated scorers/illustrative geometry | Experimental special-format engines |
| **9 Oct** | v11.74–v11.75 | Mixed-format bilateral series, then multi-team groups/fixtures and rules | Experimental commissioner feature integration |
| **9–10 Oct** | v11.76–v11.78 | Custom qualification contracts, byes planner and guided Visual Architect | Structural and UI candidates |
| **10 Oct** | v11.79–v11.80 | Searchable/paged teams, categorized World Pool browsing, first real-HTML themes | Development and browser UI candidates |
| **11 Oct** | v11.81–v11.82 | Studio/Broadcast/Pavilion and spoiler-safe UI styling | Alternative presentation candidates |
| **11 Oct** | v11.83–v11.90 | Unknown metadata, source-verified player hands, bowling families, keeper roles, aliases | Evidence-bound corrections; 400-player gate failed |
| **12–13 Oct** | B001 | 100-player two-source evidence batch, 90 bowling actions corroborated | Research dossier; ten bowling roles still unresolved |
| **14–15 Oct** | v11.91–v11.95 | Historical year affects scoring; commissioner fantasy rules separate; ODI/T20 era re-fit | Experimental, with blocked candidate(s) |
| **15 Oct** | v11.96–v11.98 | Red-ball historical survival, balanced captain default, forecast-aware declaration experiments | Full-match paired studies; not unconditional stable promotion |
| **15 Oct** | v12 RC1/RC2 | Merge full JS/CSS source, mixed-format year/rules, linked multi-edition fixtures | Integrated development candidates |
| **15–18 Oct** | v12 RC3 | Fair 4–20-team fixtures, cached old-edition commentary, save preflight | Development candidate |
| **15–18 Oct** | v12 RC4 | Forum-first BBCode, Current Squads/Atlas, squad and fixture centre fixes | Broad browser/source development, several unmerged branches |
| **18–24 Oct** | RC6/RC7/RC7.1 | Transaction safety, backup and failed-save protections | Browser/save-specific release candidates |
| **22–24 Oct** | RC8/RC8.8 | DRS, Match Centre, completion fixes and Sim All checkpoints | Playtest HTML, not certified stable |
| **23 Oct onward** | RC9 Desktop | Native Go/SQLite shell for existing HTML; Windows EXE trial | Separate native development track |
| **25–30 Oct** | P5–P8 | Pages/statistics audit, richer delivery commentary, reduced repetition, contextual presentation | Iterative page/editorial work |
| **30 Oct–2 Nov** | P9.3 | Qualification Analyst with exhaustive supported points outcomes | Tested and user-accepted |
| **3–13 Nov** | P9.4 | Decision Centre: fixtures that matter, team-specific conditional routes | Bounded development/alpha feature family |
| **3–13 Nov** | P10 | Rating/import path, real national XI cohorts, Cricsheet era and Hundred evidence | Extensive research and engine testing; some candidate gates open |
| **Late Oct–13 Nov** | RC9.1 | Full-universe JSON → SQLite guided import, rollback/readback and accidental-reset guards | Native pilot; genuine user migration gate open |
| **13 Nov** | P11 | Full-game release matrix, browser and restore acceptance methodology | Explicit pending real-environment gates |
| **13–14 Nov** | P12D | Retirement/injury law model and source-controller tests | Early development experiment |
| **18 Nov** | **Alpha 1.1** | P12E care in Quick Match, Test/FC carryover/return corrections | Full HTML alpha candidate |
| **19 Nov** | **Alpha 1.2** | Styled PlanetCricket publishers, squad/fixture/table/stat design, HEX/dropdown accents | Full player HTML; forum custom tags still admin-dependent |
| **20 Nov** | **Alpha 1.3** | P12E through standard tournament/full-XI match modes, 25-season IPL test | Full HTML alpha candidate; Sixes/Beach gap |
| **20 Nov** | **Alpha 1.4** | Opt-in portable save directory, verified rolling snapshots and folder restore | **Public PC-browser alpha** |
| **20 Nov** | Public release | Separate public download GitHub repo, playtester README, known issues and checksums | **Live public playtest** |

Dates in a row indicate the **documented milestone**, not guaranteed dates when each line of code was first written. GitHub's first PR burst in October imported existing work, so those branch creation dates are not an original authoring chronology.

## Appendix B — How the same feature changed across versions

The version register shows when things changed; this second view shows **why**. A feature can remain recognizable across months while being rebuilt internally.

| System | Original form | What broke or became insufficient | Matured approach | Public-release boundary |
| --- | --- | --- | --- | --- |
| Ball engine | v10.1 pitch/bowler outcome tables | Unrealistic dot/rotation mix, 200+ tails, all-outs, weak partial skills | V16–V22 and v11.x calibrated ball/phase models and guarded formats | Calibrated but not perfect; outliers remain |
| Player selection | Ordinary named player objects and squads | Same-name duplication, format identity leakage, historical roster confusion | Canonical identities + era/format profiles + squad memberships + source provenance | Big catalogue; accuracy/completeness varies |
| Bowling captain | Automatic phase preference | Specialist overs wasted; Quick/Tournament XIs diverged; keeper could be assigned impossible roles | Shall/Can, XI parity, legal workloads, neutral match-state captain | Weak part-time usage still a watch item |
| Domestic league | Standard league/knockout templates | Real stage graphs, divisions and promotion could not be expressed; stats scopes wrong | 30-template domestic registry, custom stages, fail-closed graph, family editions | Broad support with bespoke graph/old saves caveats |
| Tournament editing | Technical graph/CSV IDs | Easy to omit entrants, impossible round definitions, confusing creator | Guided Visual Architect and preflight; current team finder | Not every experimental builder branch fully merged |
| Match reporting | Finished summary and conventional scorecard | Spoilers, vague commentary, no partial Test records | Full chronological ledger-derived narration, session/day views, Match Centre | Wording still limited by what engine knows physically |
| Forum exporting | Basic BBCode/plain match posts | Hard to post a stylish entire league; site-specific tags unavailable | Standard BBCode fallback + optional styled PlanetCricket match/squad/table/fixture/stat posts | Custom style awaits external admin |
| Saved universe | localStorage with last 100 matches retained | Historical records too large, browser-profile dependence, failures risk silent loss | IndexedDB, compressed/JSON backups, staging/readback, portable folder snapshots | Real cross-PC cold-restart test still open |
| Historical simulation | Player-era labels but often modern match environment | Older T20s scored like modern fixtures; Test sessions/draws off | Year-aware outcome patterns, rule/year separation, 3–5-day red-ball decisions | Source-matched calibration remains incomplete |
| DRS | No integrated review path; source evidence prototype | Must rule during the ball, track review budgets and scorecards | Configurable DRS in later RC match/scorecard/commentary flows | Not a professional ball-tracking physics system |
| Qualification | Table positions only | No explanation of mathematical routes or important fixtures | P9.3 points analyst → P9.4 Decision Centre | Conservative on NRR and unsupported stages |
| Injuries | No match-day retire-hurt/out coverage | What if the team has eleven players; who fields/bowls; can batter return? | P12 rules controller → Quick Match → standard tournaments | Specialty six-player games incomplete |
| Windows | One local HTML file | Users want PC durability and portable history | Separate Go/SQLite pilot plus Alpha 1.4 browser folder export | Public release is HTML, not native EXE |

## Appendix C — Audit and historical gaps

This timeline draws from the dated June archived v11.6 HTML, recorded early conversations, July/August archive inventories, September calibration reports, October validation documents and **all 89 current branch tips / 68 available PR records** examined in the accompanying validation audit. Branch-tip inventory is not a forensic reconstruction of every deleted intermediate commit.

The surviving evidence is **much richer after September**. Do not infer missing exact May dates, intermediate summer versions or author attribution just because the narrative is chronological. Never confuse a proposal, prototype, scripted logic test, full cricket match, synthetic controlled fixture and actually distributed playable build.

**Three questions remain distinct:**
1. **Was the feature designed or requested?**
2. **Was it implemented and tested in a particular development candidate?**
3. **Is it present, reachable and reliable in the exact public Alpha 1.4 game?**

The third is the highest bar. This history records work in all three categories, but it does not claim a failed or partial development branch was magically completed simply because the project later shipped an alpha.


## Evidence and dating notes

The main October/November dates are grounded in dated **v11.63–v11.98 validation reports**, **v12 RC/P8–P12 development records**, and the **89 current branch-tip / 68 PR** inventory of the private HFCricSim repository. The early June foundation is recorded in historical conversations and an engineering dossier; the July/August dates come from archival source-file inventory. The September-to-November testing history is separately documented in **[Validation History](./VALIDATION_HISTORY.md)**.

The private branch collection includes experiments, source-only drafts, rejected candidates and release-transport branches; neither a pull request date nor a version label alone proves deployment. Individual October branches often appeared on the same date because earlier work was imported into GitHub—this is **not** proof all those features were invented in one day.

**Further reading:** [Alpha 1.4 download](https://github.com/MaqdoomAli/CricSim-Releases/releases/tag/Simulator) · [Player guide](./README_PLAYTESTERS.md) · [Validation and stress tests](./VALIDATION_HISTORY.md) · [Known issues](./KNOWN_ISSUES.md).

*Compiled 20 November 2026. The document is a feature-evolution history, not a new test run, a claim of sole manual authorship, or an assertion that all experimental branches were merged into Alpha 1.4.*
