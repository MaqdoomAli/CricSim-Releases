# 🏏 CricSim v12 Alpha 1.4 — Playtester Guide

**Build:** Alpha 1.4 — Portable Save Folder · **Status:** Public playtest candidate, not a certified stable release  
**Game:** Offline HTML cricket simulator for desktop browsers  
**Recommended browser:** Latest Google Chrome or Microsoft Edge on Windows/macOS/Linux  
**Important:** Make backups. Alpha versions can contain bugs or change how saved games are handled.

Welcome to CricSim! Create squads, manage tournaments, and simulate cricket ball by ball. It includes quick matches, multi-stage competitions, player ratings, detailed scorecards, commentary and long-running statistics. Your feedback will shape the next release.

## 1. Install and start playing

1. Extract the ZIP to a normal folder on your computer. Do **not** try to play the HTML directly from inside the ZIP.
2. Open **`CricSim_v12_ALPHA1_4_Portable_Save_Folder.html`** in Chrome or Edge. Internet access is not required for normal offline play.
3. Start at **Home**. Choose **Quick Match** to simulate a single match, or **Tournament** to create a season.
4. If this is your first time, use the built-in squads. If you have an existing CricSim universe, **back it up before importing, restoring, or switching versions**.
5. Open **Portable Save Folder** on Home and set up a dedicated save directory before you play for long.

**Avoid private/incognito browsing, clearing site data, changing browser profiles, or deleting the HTML and save folder without making an export first.** The regular game save is stored in your browser; the portable folder is your transferable recovery copy.

## 2. The game in five minutes

### Play a Quick Match

1. Click **Quick Match** in the left sidebar.
2. Choose a match format, two teams and their squads.
3. Leave pitch, weather, year, DRS and advanced options at their defaults for your first game, or experiment with them.
4. Click **Simulate Match**.
5. Read the match result, innings scorecards and ball-by-ball commentary. Check whether the result, bowling figures and story agree.

### Run an IPL season

1. Open **Tournament** and create a new competition/edition.
2. Choose the **IPL-style** format and select the ten included IPL 2026 team squads (or your own teams).
3. Review match conditions and optional **Injury & Retirement** settings **before** starting the edition.
4. Simulate fixtures individually to inspect scorecards, or use **Sim All** and its progress indicator to complete the season.
5. Review the league table, playoffs, champion, batting/bowling leaders and individual match reports.

The IPL 2026 rosters are a game dataset. Simulated seasons are fictional, not predictions of actual IPL results.

### Manage your players

Use **Teams & Squads** to manage team membership and player roles, **Players** for player identities and ratings, and **World Pool** to explore supplied teams and player profiles. The **Archetypes** page changes playing styles; changes to archetype defaults affect newly created players, so inspect existing player attributes separately.

The broad skill scale is **1–20** for batting and bowling. Batting and bowling should be rated independently. Upsets are possible: a moderately rated player can have a remarkable season, but you should tell us if a weaker player dominates repeatedly.

## 3. Save your entire CricSim universe — Alpha 1.4's new feature

Your universe includes teams, squads, players, tournaments, fixtures, played matches, scorecards, historical results, player statistics and relevant game settings.

### First-time folder setup

1. On **Home → Portable Save Folder** (or the **Save Folder** shortcut at the top), click **Choose Folder**.
2. Select an **empty, dedicated folder**, such as `Documents/CricSim Saves`.
3. When CricSim confirms it is a new folder, approve **Start automatic snapshots**.
4. Play normally. The browser continues to hold the working save; CricSim also creates folder snapshots after saved changes **while permission is available**.
5. Click **Save Now**, then **Verify Folder**, before closing the browser or copying the folder. Look for **Folder verified** and a recent save timestamp.

A normal save folder has this shape:

```text
CricSim Saves/
├── CricSim-Saves.json        (points to the latest verified snapshot)
└── snapshots/
    ├── CricSim-...json      (complete save)
    ├── CricSim-...json      (older recovery point)
    └── CricSim-PreRestore-...json  (created before restoration, when needed)
```

The game retains **up to 12 rolling indexed snapshots**. Larger universes can consume significant disk space. Keep the entire folder together; **do not rename or manually edit the files**.

### Continue after restarting the computer

1. Open the same CricSim HTML in the same browser/profile.
2. Confirm your teams and tournament progress are present.
3. Check **Save Folder**. Browser permission may need renewing: choose the same folder again if prompted.
4. Click **Verify Folder** to make sure your backup is current. If the browser's working save differs from the folder, **do not overwrite either version without checking which is newer**.

### Move to another PC or a different browser profile

1. On the original computer, click **Save Now** and ensure **Folder verified**.
2. Close the game and **copy the entire CricSim Saves folder** to the new computer (USB or an offline cloud-copy transfer).
3. Copy the Alpha 1.4 game HTML as well; the saved folder contains your *game data*, not the game itself.
4. Open the HTML in Chrome/Edge on the new PC, click **Choose Folder**, and select the copied folder.
5. Click **Restore from Folder**, read the displayed save date and counts, and confirm **only if this is the universe you want to load**.
6. The game creates a **pre-restore safety snapshot** before replacing the working browser universe, then reloads. Check the teams, fixtures, match scorecards and player records before continuing.

**If you choose a folder containing an existing save, CricSim does not automatically overwrite or merge it.** Restore it first, or explicitly choose to keep the current browser data after reviewing the warnings.

**Do not run two active copies of CricSim against the same live or cloud-synced save folder.** Use **Save Now → copy folder → restore** instead. If the game reports a conflict or failed verification, stop saving into that folder, preserve both copies and report the issue.

### Folder picker unavailable?

Some browsers or offline-file configurations do not allow direct folder access. Use the existing **Backup Universe** / **Import or Restore Universe** JSON options instead. Keep a separate copy of every important backup. **Never reset your browser data to troubleshoot without making a backup.**

## 4. Injuries and retirement — what to test

Alpha 1.4 retains the P12E rules added in Alpha 1.3 for standard eleven-a-side Quick Match and tournament engines.

- **Retired hurt:** The batter is not dismissed and may return when fit and eligible.
- **Retired out:** Counts as a dismissal without crediting a wicket to a bowler; return eligibility depends on the selected playing-condition rules.
- **Injured bowler:** When a replacement is legally available, another eligible bowler finishes the over; a reserve fielder can be selected automatically.
- **Exactly 11 players in the squad:** Spontaneous *bowler* injuries are suppressed to avoid impossible bowling/fielding replacements; batting injuries remain possible.
- **Rare automatic events:** Optional rather than compulsory; spontaneous injuries are limited to one per match.

**Quick Match:** Find **P12E Injury & Retirement** under match setup. You can choose an ICC/MCC/BCCI rules profile and optionally schedule a manual incident.  
**Tournaments:** Review the medical options in tournament creation/fixture setup. Check an affected scorecard to see whether the innings and player records remain consistent.

Automatic injuries are **off by default**; you must enable them for injury-frequency testing. A game with no injuries is not necessarily a bug. Special **Hong Kong Sixes** and **Beach Cricket** match engines do **not yet** have complete P12E support.

## 5. Other features worth exploring

- **Formats:** T20, ODI, Test, first-class, T10, The Hundred and supported competition variants.
- **Match conditions:** Pitches, weather, era-based scoring environment, rain scenarios and optional DRS.
- **Competitions:** IPL-style leagues with playoffs, standard leagues, knockouts, international/tour series and more advanced custom structures.
- **Statistics:** Match and competition batting/bowling statistics, records and player histories; check that totals match the scorecards.
- **Commentary:** Ball-by-ball delivery descriptions, including the actual outcome for the delivery. See whether the narrative makes cricketing sense.
- **Forum Publisher:** Export standings, squads, fixtures and statistics as PlanetCricket BBCode, with optional colours. Custom-styled codes need the forum administrator's matching BBCode setup; plain BBCode is the fallback. Match reports are also available through the game's built-in forum report workflow.

Some areas are advanced. You are not expected to test everything in one sitting.

## 6. Suggested playtest challenges

Choose any one of these—or complete several if you enjoy testing:

| Challenge | What to do | What to look for |
|---|---|---|
| **A. First match** | Simulate two T20 matches with different pitches. | Plausible scores, legal overs, sensible scorecards and commentary. |
| **B. IPL manager** | Complete an IPL season with default player ratings. | Tables, playoff progression, champions, match history and realistic award winners. |
| **C. Hidden gem** | Find a batter rated 12/20 or below, or bowler rated 12/20 or below, who has a great season. | Report their rating, batting position or bowling role, and actual season figures. |
| **D. Medical rules** | Enable rare injuries; try one controlled retirement scenario. | Correct wicket accounting, field replacements, return rules and scorecard notes. |
| **E. Long-term save** | Play at least five tournament fixtures, click Save Now, close/reopen, and verify records. | No missing squads, fixtures, completed matches or statistics. |
| **F. Transfer test** | Copy your save folder to a separate browser profile or another PC. | Restoration succeeds and matches/scorecards/statistics are unchanged. |
| **G. Forum sharing** | Export a squad, standings table and statistics with two different colours. | Clear headings, correctly formatted tables, readable text in a forum preview. |

**Please report strange results, not just crashes.** For instance, repeated 600-run seasons by a low-rated player, a low-rated part-time bowler regularly winning the wicket award, impossible over allocations, missing runs, or a table that disagrees with match results are valuable findings.

## 7. How to report a bug

Copy this template into your feedback message:

```text
CricSim version: v12 Alpha 1.4
Browser/OS: (e.g., Chrome on Windows 11)
Area: (Quick Match / IPL / Save Folder / Statistics / Forum / other)
What I was trying to do:
Steps to reproduce:
Expected result:
Actual result:
How often: (once / sometimes / every time)
Any relevant teams, players, ratings, fixtures or match scores:
Error message or screenshot, if available:
Did the game save successfully before/after the issue?:
```

**For performance or unusual cricket results:** include the competition format, simulation year, number of matches played, player ratings, batting/bowling roles and affected statistics. A screenshot of the scorecard or table is useful. If you can reproduce the issue, describe the shortest sequence that triggers it. **Back up before attempting risky reproduction.** Only share save files if you are comfortable sharing the names and records inside them.

## 8. Known limitations and playtest safety

- This is an **alpha**. Your progress matters: use **Save Now** and periodically export a **Backup Universe JSON** separately.
- Browser storage is not a cross-device service. A new PC does **not** automatically see your old game until you copy and restore its save folder or import the JSON backup.
- Direct folder access is best supported in modern desktop Chrome/Edge. Browser permission may need renewal on restart; some `file://` browser settings can prevent folder access.
- The **25-season IPL stress test** on the Alpha 1.3 engine completed 1,850 matches, but it does not guarantee that every rating, competition type or rare scenario is correct. Alpha 1.4 changed the saving layer, not the underlying match engine.
- Standard-format medical rules are included; **Hong Kong Sixes and Beach Cricket** remain exceptions.
- Some PlanetCricket styles require forum-side BBCode registration. If in doubt, start with plain BBCode.
- Real physical PC-to-PC transfer, browser cold-reopen and live PlanetCricket installation still need more user testing. Please tell us if you complete any of these successfully or encounter an error.

### Most important playtester rule

**If your matches or squads seem missing, do not create new data, reset the browser, or overwrite the folder immediately.** Check the selected browser profile and game file, inspect **Save Folder** status, and preserve your existing browser export and portable folder for recovery.

---

Thank you for helping test CricSim. Have fun experimenting—and please tell us when something feels *wrong as cricket*, even if the game technically runs.
