# 🏏 CricSim v12 — Alpha 1.4 (Public PC Playtest)

**A cricket simulator for people who want to build squads, stage competitions and follow the actual cricket—not just see a final score.**

**[⬇ Download CricSim Alpha 1.4 Playtester ZIP](https://github.com/MaqdoomAli/CricSim-Releases/releases/download/Simulator/CricSim_v12_Alpha1_4_Playtester.zip)** · [Release page](https://github.com/MaqdoomAli/CricSim-Releases/releases/tag/Simulator) · [Playtester guide](./README_PLAYTESTERS.md)

**[📖 Development timeline: from May to Alpha 1.4](./FEATURE_DEVELOPMENT_TIMELINE.md)** — the complete feature history, decisions, replacements and experimental work.

**Quick start:** Extract the game ZIP, open **CricSim_v12_ALPHA1_4_Portable_Save_Folder.html** in Chrome/Edge, and play offline. **Make backups.** This is an alpha, not a certified stable release.


## What is CricSim?

**Build the cricket. Run the season. Read what actually happened.**

CricSim is an **offline cricket simulation game for PC**, focused on match and competition management rather than controlling a bat in 3D. You choose teams, build squads, set playing conditions and competition rules; the engine plays the contest **delivery by delivery**, creates a detailed scorecard and keeps the history.

I've been working on this since May, and Alpha 1.4 is the first version I feel comfortable sharing with other cricket fans. It's not flawless. I want to see what you can build with it—and where the simulation breaks down when somebody other than me puts it under pressure.

### 1. From a one-off T20 to a five-day Test

- **Formats:** T20, ODI, Test, first-class, T10, The Hundred and custom-over matches.
- **Quick Match:** Pick two squads, set the context and simulate the whole contest. Or jump into a scheduled tournament fixture.
- **Pitch and ground:** Choose flat, green, spinning, bouncy, slow/low or neutral surfaces, plus venue, weather, toss and day/night conditions.
- **Cricket through the years:** Set a simulation year to change the era's scoring environment. Mix eras and fantasy matchups without being restricted to recreating a historical result.
- **Rule options:** Free hits, tied-match/Super Over rules, optional Impact Player in applicable T20 contexts, and configurable DRS modes and unsuccessful-review allowances.
- **Rain and revised targets:** Script a rain interruption or enable supported dynamic weather. The revised target uses the **published Standard Duckworth/Lewis system**—it is *not* the licensed modern Professional DLS used by broadcasters.
- **Player care:** Standard full-XI matches include opt-in, deliberately rare injuries, retired hurt, tactical retired out, eligible batting returns and injured-bowler contingencies. An eleven-player squad does not suffer spontaneous bowling injuries with no eligible substitute.

The game is a simulation sandbox, not a claim to forecast real-world results. Cricket has noise, and exceptional performances can happen even to imperfect players.

### 2. Your squads, your ratings, your universe

CricSim lets you create and edit teams, maintain different squads for a club or country, manage selection and batting order, set bowling eligibility and assign wicketkeeper/captain roles. There are separate **batting and bowling skill ratings on a 1–20 scale**, archetypes, individual attributes and matchups.

Crucially, the player library separates **one cricketer's identity** from different **format/era performance profiles**. A player can have a Test rating from one period and a different T20 profile while still being recognised as the same individual.

The compressed World Pool inside this *actual* Alpha 1.4 file contains:

| Data in the built-in catalogue | Records |
| --- | ---: |
| Player identities | **10,877** |
| Format- and era-specific performance profiles | **72,302** |
| Team records | **415** |
| Squad snapshots | **3,295** |
| Tournament catalogue entries | **342** |

These are catalogue counts, **not** a claim that every historical squad is perfect or that every catalogue entry represents a fully playable competition. The library is a starting point that users can edit, extend and correct.

### 3. Run your own IPL, World Cup, domestic season—or design a competition

- **IPL-style:** League table, fixtures, top-four qualification, eliminator, qualifiers and final.
- **Other tournament paths:** Round-robin leagues, knockouts, World Cup-style groups, Swiss formats and further multi-stage structures.
- **Competition Architect:** Create linked stages, fixture structures, points and ranking rules, progression and qualification routes.
- **Domestic structures:** The catalogue includes a range of first-class and limited-overs competition templates with specific stage graphs rather than simply treating everything as a generic round robin. Coverage and roster completeness vary.
- **Season continuity:** Track fixtures, completed results, points tables, player statistics, records and tournament editions. The Sim All feature can advance a run of fixtures with progress feedback.
- **Qualification intelligence:** The Tournament Hub's Qualification Analyst separates mathematically secure, still-possible, tiebreaker-dependent and eliminated positions where the available schedule allows it. The Decision Centre highlights fixtures that matter and routes teams still need. It does not invent future scores, NRR guarantees or made-up probabilities.

You can play with established real-world squads or make up your own cricket structure. That's a big part of why I built this.

### 4. The match doesn't disappear when you press Simulate

This isn't a simulator that prints **182/6 beat 179/9** and calls it a day.

There are **full batting and bowling scorecards**, dismissals, extras, fall of wickets, innings details, individual bowling figures and **ball-by-ball commentary** with batter, bowler and delivery outcomes. The Match Centre also presents the contest in a more readable way: chronological developments, significant moments and a match story constructed from the actual events.

You can revisit matches instead of losing them as soon as the screen changes. Commentary has reader-focused and spoiler-conscious options so following a contest can be more than immediately revealing the winner.

The statistics and records pages provide tournament leaders, batting and bowling analysis, player and team records, standings and histories. I'm especially interested in whether these numbers *feel* right over a whole season—not just whether one scorecard looks believable.

### 5. Share a league on PlanetCricket

CricSim includes exports intended for forum league threads: **complete match reports and scorecards, squads, tables, fixtures and player statistics**. The newer publisher also lets users select a colour theme or supply a HEX accent.

**Important:** The enhanced visual output depends on new custom BBCode being installed on PlanetCricket. Those forum-side changes have been submitted, but their availability is outside this game release. The **standard/plain BBCode output** is provided in the meantime.

### 6. Save an entire cricket world—and take it to another PC

Alpha 1.4 introduces an **opt-in Portable Save Folder**. Rather than relying only on a particular browser profile, it can create verified, rolling snapshots of the game universe—teams, players, squads, tournaments, played matches, scorecards, statistics and settings—in a folder you select.

Users can copy that folder to another computer, select it in CricSim and restore from it. The original browser-local save system remains, and a **full-universe JSON export/import** is available as a fallback.

**Please back up your universe.** Browser folder permissions sometimes need renewing, and real-world close/reopen and PC-to-PC transfer still need broader testing. This is an alpha.

## Download, extract and play

1. **[Download the Alpha 1.4 Playtester ZIP](https://github.com/MaqdoomAli/CricSim-Releases/releases/download/Simulator/CricSim_v12_Alpha1_4_Playtester.zip)**.
2. Extract the ZIP to a normal folder. **Don't open the HTML from inside the ZIP.**
3. Open **CricSim_v12_ALPHA1_4_Portable_Save_Folder.html** in current desktop **Chrome or Edge**.
4. Try Quick Match, create an IPL tournament or explore the built-in World Pool. Normal gameplay works **offline**—there's no installer, account or live server.
5. Read **START_HERE_PLAYTESTERS.md** inside the ZIP before attempting save transfers or restoring old universes.

## Testing, calibration and why this is ready for public playtesting

The **25-season IPL test is only the most recent run**, not the whole testing history. Earlier September calibration alone ran **100 IPL seasons (7,400 matches)** and then a distinct **90-season IPL leader audit (6,660 matches)**, followed by a **1,200-match ODI stress test**. CricSim has been repeatedly exercised across different formats, versions and parts of the game:

| Documented run | Workload | What it actually tested |
| --- | --- | --- |
| **V16 — 100-season IPL calibration** | **7,400 full matches / 14,800 innings** | Ball-by-ball scoring distributions and player/season realism; identified excess all-outs and too few 200+ totals |
| **V18 — 90-season IPL leader audit** | **6,660 matches across 90 seasons** | Orange/Purple Cap leaders, hundreds, 50–100 conversions, high-scoring team totals and individual hot streaks |
| **V19 — ODI stress and Cricsheet comparisons** | **1,200 seeded ODIs** | Phases, centuries and bowling-haul frequencies; same synthetic four-team pool, not every ODI era |
| **v11.93 — 23-year era benchmark** | **4,554 first innings + 138 full matches** | Controlled T20/ODI/Test scoring comparisons across 2004–2026, not observed historical match replay |
| **V22 — ODI tournament calibration** | **30 complete 45-match ODI tournaments / 1,350 games** | Twelve-team scoring and tournament-leader distributions; fifties and all-outs remained slightly high |
| **Phase 2U — pitch and domestic calibration** | **2,240 simulated first innings** across seven surfaces, plus 77-match One-Day Cup and 63-match Sri Lankan FC seasons | Compared pitch ordering and format-specific run/wicket responses; BPL leader-tail imbalance remained |
| **v11.96 — era-specific T20 leader study** | **48 synthetic 70-match leagues / 3,360 fixtures** | Different 2006, 2015 and 2026 league leader tails; not historically matched IPL calendars |
| **v11.97 — red-ball captaincy** | **864 real-engine complete matches across the old and new versions combined** | Three-, four- and five-day outcomes, XI depth and weather, with substantial draw-rate sensitivity |
| **v11.66 six-format ball-engine stress** | **998 matches, 54 editions, 2,081 innings and 313,975 legal deliveries** | Real ball-by-ball T20, T10, Hundred, ODI, Test and first-class scorecards, bowling figures, extras, records and tournament-stat reconciliation; no mismatches in the audited invariants |
| **v12 ten-team IPL stress** | **740 full matches across 10 editions**, then **148 in two corrected-candidate editions** | Real IPL-style league and playoff progression; exposed and fixed a home/away fixture imbalance; 100 independent fixture-seed fairness checks passed |
| **Alpha 1.3 final IPL stress** | **25 full seasons / 1,850 matches**, including 100 playoffs | Zero simulation exceptions in that run; rating outliers, bowler usage and limited injury behaviour inspected |
| **Long-term domestic competition stress** | **30 County Championship editions / 3,780 controlled fixtures** | Promotion/relegation, season identity, stage progression and archive continuity—not 3,780 real ball-by-ball matches |
| **Cricsheet ODI A/B calibration** | **660 schedule-matched simulations** against a **66-match real ODI cohort** | First-innings mean improved from 238.27 to 248.57 versus 251.92 observed; chase outcomes and all-out frequency also moved closer to observed cricket |
| **Specialty formats and save integrity** | **750 six-player exhibition matches** in an earlier specialty-format audit; **308 matches / 31 editions** in a backup test | Separate rule/ledger regressions and lossless in-memory save fidelity, **not** a complete cross-PC restore certification |

The underlying development audit examined **all 89 current private-repository branch tips** and **68 PR records**, tracing inherited reports so that repeated suites, matched seeds, controlled fixtures and researched real-world match records aren't incorrectly marketed as new gameplay simulations. The [full evidence record](https://github.com/MaqdoomAli/CricSim-Releases/blob/main/VALIDATION_HISTORY.md) also covers V22 ODI tournaments, Phase 2U domestic/pitch tests, red-ball captaincy matrices, forum publishing, source validation, World Pool provenance and save failures.

There have also been focused runs for Test weather, declarations and captaincy, DRS, rain interruptions, over limits, qualification routes, match-versus-tournament parity, statistics and save failure handling. **Some tests caught genuine bugs and limits**: fixture fairness, a roughly 504 MB full-universe export timeout, and incomplete browser cold-restart coverage.

I have published a [**detailed, version-by-version validation history**](https://github.com/MaqdoomAli/CricSim-Releases/blob/main/VALIDATION_HISTORY.md) with exact scopes and caveats. **These are historical tests from different builds, not a claim that every suite was rerun on this exact Alpha 1.4 download or that the counts should be added together as unique matches.** The full history also distinguishes 3,780 controlled County results, 23,637 stubbed qualification fixtures, 408-per-engine Hundred matched-XI runs, old-vs-new A/B tests, and Cricsheet real-world source data.

The latest IPL run also raised legitimate questions: some **12/20-rated batters** had outstanding 500–650-run seasons and a few **9–10/20-rated part-time bowlers** received substantial workloads. I don't want to arbitrarily nerf them after an odd season. That's exactly the kind of behaviour I want independent playtesters to examine.

**What I'd love you to try:** Run complete seasons, challenge the ratings, test weak and strong teams, look at bowling changes and chase behaviour, inspect match commentary, finish multi-stage tournaments, and **try saving, closing and reopening the game**. When something is wrong, give the browser, build number, format, player ratings, fixture and scorecard—or a copy of the steps to reproduce it. Please keep your personal save files private unless you're comfortable sharing them.

**Known alpha limitations:** Enhanced PlanetCricket BBCode requires forum-side installation; Hong Kong Sixes and Beach Cricket do not yet have the complete injury treatment of the standard full-XI formats; saved-universe migration needs wider field testing; not every catalogue roster is complete.

**I want you to enjoy it, but I also want you to tell me where it stops looking like cricket. That feedback is the reason this release exists.**


---

[Release notes](./RELEASE_NOTES.md) · [Known issues](./KNOWN_ISSUES.md) · [Validation history](./VALIDATION_HISTORY.md) · [Latest IPL test](./IPL25_STRESS_REPORT.md) · [Report a bug](./REPORT_BUG.md)

**The private developer repository and the PlanetCricket administrator-installation materials are not part of this public release.**
