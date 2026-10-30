# Touchline 27 v0.3 — Linux Live Editor

A native Linux desktop editor for the locally inspected Football Manager 27 Steam build running through Proton. Built and tested on Arch Linux. Independent software, not affiliated with FMRTE, Sports Interactive or SEGA.

## Standalone Arch Linux executable

Use `Touchline27-Linux-Arch` from the outputs folder. It is a single executable containing Python, Qt, the icon and all editor modules. No extraction, terminal command, Python installation or FUSE is required on the tested Arch Linux desktop. It can be moved or renamed independently of this source folder. Its executable permission is already set on this computer.

The standalone app saves settings, favourites, journals, game cache and startup logs in `$XDG_DATA_HOME/touchline27`, normally `~/.local/share/touchline27`. Its embedded assets are unpacked temporarily on launch; persistent state is kept outside that temporary folder. This differs from the source version, which uses its adjacent `state/` directory. If startup fails, details are recorded in `startup.log` inside the standalone data directory.

The desktop shortcut in this source folder points to the standalone executable's current output location. Launching the executable directly avoids path-sensitive shortcut files.

## Start

Double-click `Touchline27.desktop` or run:

    ./launch.sh

Python 3 and PySide6 are required. They are already present on the machine where this was built. On another machine, install locally without changing system Python:

    python3 -m venv .venv
    .venv/bin/pip install -r requirements.txt
    ./launch.sh

The `.desktop` file contains the current absolute installation path. If you move this folder, update its `Exec` and `Icon` entries. `editor.py` is the earlier raw-memory prototype; use `app.py` or `launch.sh` for Touchline.

## Everyday use

1. Save a separate backup in FM. The app also offers **Back up a save** to copy a selected `.fm` file.
2. Start FM27, load your career, and select **Load Game** in Touchline. It loads automatically when a running game is detected.
3. Choose **Clubs** or **Players**, search a name/unique ID, and double-click the result.
4. Edit the named fields and choose **Save changes** (`Ctrl+S`). Review the exact changes, then apply.
5. Check the outcome in FM. Save inside FM to retain live edits in your save file.

Use **Rescan** after switching careers. The same-process cache accelerates reopening; a game restart requires discovery again. Switching careers is not automatically detected. Every selected record and write has process/record identity checks, but those do not replace rescanning after loading another career.

## Available features

- Search clubs, players and staff by name or UID; filter players by club, nation text, CA and PA.
- Independent record tabs with named pages, favourites, sortable results and CSV export.
- Club cash balance, remaining and seasonal transfer budgets, weekly wage budget, maximum individual weekly wage, and transfer-revenue percentage.
- Club squad lists, including first-team filtering, opening player profiles, mass edits, and recovery actions on selected/filtered players.
- Player current/potential ability, home/current/world reputation.
- Technical, mental, physical, goalkeeper, hidden and personality attributes, plus position familiarity. Attribute presentation uses 1–20; ability uses 1–200.
- Player/coaches are included in player discovery. Staff coaching, mental/recruitment, medical/analysis, personality, ability and reputation editing is available for mapped standalone staff records.
- Full-contract weekly wage and expiry-date editing for mapped players and staff. A calendar control preserves the game’s date flags. Basic and loan contract types remain read-only.
- First-team club reputation editing.
- Relative mass changes: set, increase, decrease, minimum and maximum, limited to valid field ranges.
- Reusable player/staff presets: capture a profile, create group/single-field actions, import/export Touchline JSON, and apply to selected records after a preview.
- Read-only comparison of 2–4 selected players.
- Team tactical-knowledge tools: inspect mapped players and maximise all existing learned instruction and formation/role entries to 100%, with preview and undo.
- Injury descriptions and recovery-duration inspection; experimental recovery-to-zero actions for individual players and squads.
- Live field refresh every five seconds while a profile has no pending edits. Pending edits are preserved.
- Optional currency calibration: enter an exact raw amount from the field tooltip and the exact corresponding FM amount, then name the display currency. Money uses the calibrated conversion; wages remain weekly. Unedited raw bytes are preserved even when displayed money is rounded.
- Preview, per-batch history, guarded undo, local before/after journals, and best-effort rollback on write failure.
- Advanced numeric memory search and narrowing for unmapped fields, separated from ordinary named editing.


## New in v0.3

Close and reopen Touchline to load the update. Your current running window is left intact so you can finish or undo existing edits before closing it. Favourites, currency settings, game cache and journals are preserved.

- **Staff → profile** now has coaching, recruitment, medical, personality and ability pages.
- **Player/Staff → Contract** exposes mapped full-contract weekly wages and an expiry-date calendar.
- **Club → Club settings** exposes first-team reputation.
- **Club → Tactical knowledge** provides the squad knowledge action described below.
- **Selection tools** in player/staff results offers relative mass edits and presets. Player results also offer comparison and tactical knowledge.
- **Presets** in the sidebar manages reusable actions. Select player/staff rows before opening the library, or use **Apply preset** on a profile.
- **Save preset** on a player/staff profile captures its displayed attribute values, including pending attribute edits. It saves a definition; it does not write to the game. Contract wages and dates are excluded from attribute presets.

The five supplied presets are Physical boost +1, Potential at least 170, Position familiarity 16, Coaching at least 16, and Medical team at least 16. Relative operations respect each field’s limits. Import accepts only recognised semantic fields and operations; arbitrary memory addresses and executable expressions are never used.

## Full tactic knowledge button

Open a club, choose **Tactical knowledge**, choose **First team** or **All club squads**, and select **Full tactic knowledge (100%)**. With no rows selected, it targets the chosen squad; selecting rows narrows the target. Review the changes and apply. The same action is in player results under **Selection tools**.

The action sets every **existing learned instruction and formation/role familiarity entry** for the targets to 10,000 (100%) and marks the game’s knowledge cache for refresh. It preserves instruction keys, formation keys and ownership links. It does not change player ability, buy-in, cohesion or the current tactic itself, and it does not claim to force FM27’s overall tactical-effectiveness score to 100%.

This implementation does **not allocate new entries for entirely untrained tactics**. Missing, incomplete, out-of-range or changed records cause the whole requested batch to be refused; there is no silent partial-squad write. Train an entirely new tactic in FM first, then use the action again once data exists. The panel’s percentage is an average of stored learned entries, not a reconstruction of FM’s current-tactic gauge.

The storage was checked against FM27’s native maximise-understanding implementation: six packed instruction collections use a 14-bit value, and formation/role rows use a 16-bit value at row offset 14. Three exact native code anchors are checked in addition to the module fingerprint. A live read-only check covered 25 first-team players and planned 338 fields; no development writes were made to FM. Write/undo behavior and preservation of packed keys were verified in a controlled fixture process. The effect on FM’s current tactic display still needs checking after applying.

## What “Heal squad injuries” does

This is an **experimental recovery-duration edit**, not a fully verified equivalent of FMRTE's Heal feature. It sets decoded injury light/full-training recovery durations to zero. It does not delete injury objects, alter game code, or invoke game functions. FM must process its next medical update; actual injury removal has **not** been verified in-game during development.

Condition, fatigue, match sharpness, morale, bans and registration are separate and are **not edited**. Recovery records must pass structure, ownership, date, description and duration checks. Unrecognized records are skipped. The preview identifies affected players and injuries. A club action uses selected squad rows, or the currently filtered squad list when nothing is selected.

## Compatibility and limits

Named layouts are enabled only for native module SHA-256:

    bc940e472418fc5c214208d7cf30fecf600f689b6f9d71c11ba19a2df0fe057f

RTTI discovery derives vtable addresses from the installed module; addresses are not hardcoded. Some staff classes only have secondary vtables, which are translated back to their complete object addresses. Individual layout offsets are still build-specific and live in `layout.py`.

The tested live session yielded 50,355 clubs, 120,349 named players including player/coaches, and 63,416 named staff. Only 1,033 clubs had a full finance object in this career; other clubs expose the base balance record. The app does not invent missing budget structures. Counts depend on the career and loaded database.

Named scalar layouts were checked with read-only game inspection and plausible known player/club records. Memory write/readback, undo and recovery edits were tested against a controlled fixture process. **No FM27 game values were modified during development. Actual game behavior after applying edits remains to be verified by checking FM.**

Not implemented: fitness/sharpness/condition restoration, permanent injury removal, contract clauses/loan/basic-contract editing, transfers/swaps, club facilities, stadiums, kits, competitions, freezing values, or FMRTE-format presets/shortlists. This is a substantial experimental editor, not full FMRTE feature parity.

The app reads and writes `/proc/<pid>/mem` using the current account. It does not request root, change kernel security settings, inject DLLs, bypass licensing, or patch executable code. If access is denied, it reports that error. Discovery can take about a minute and reads several GiB of memory; repeated startup normally uses a validated cache.

## Undo, backups and local state

**Changes** lists applied batches. Select one and use **Undo selected batch**. With no selection, `Ctrl+Z` targets the latest unapplied-to-undo batch. Undo checks that the bytes still equal the edited values and that record identities/ownership still match. FM can change values while running, so restoration is not guaranteed. A multi-field batch is not an atomic transaction against the game thread; on failure the app attempts guarded rollback.

State lives beside the app in `state/`:

- `index-cache.json`: game index for the current process, containing game names and transient addresses.
- `preferences.json`: favourites and currency conversion.
- `presets.json`: reusable semantic player/staff presets; no game addresses are stored in presets.
- `history/`: durable JSONL journals with original/edited bytes and identity checks.

Journals survive closing the app, but UI undo currently only covers the current attachment session. Reloading the index starts a new history view. Journals are diagnostic/recovery evidence, not automatically replayed across game restarts. Use a separate FM save backup for durable recovery.

## Validation

Run the included controlled-process and offscreen GUI tests:

    python3 -m unittest discover -s tests -v

Tests cover budget/attribute plans, recovery-duration plans, real process-memory writes/readback, undo, identity/stale-value rejection, partial-write rollback, search cancellation, build/layout guards, currency conversion preserving untouched bytes, UI staging and result filters. The current suite has 28 tests, including staff attribute writes, contract date/flag preservation, reputation ownership, preset validation, and tactical-knowledge packing/code-drift guards. Tests do not modify FM.

## Research and credits

The interface follows workflows described in FMRTE's public documentation: [interface tour](https://docs.fmrte.com/getting-started/interface-tour/), [search](https://docs.fmrte.com/scouting/search/), [club finances](https://docs.fmrte.com/clubs/finances/), [player attributes](https://docs.fmrte.com/players/attributes/), and [condition versus injury recovery](https://docs.fmrte.com/examples/condition-preset/), and [match preparation](https://docs.fmrte.com/clubs/match-preparation/).

[FM AI Assistent](https://github.com/JelmerBouma1985/fm-ai-assistent) provided public FM26 memory-layout research used as hypotheses for local FM27 inspection. This app uses an independent Python/Qt implementation; it does not bundle that project's source. FM27 native RTTI, names, ownership links, secondary-subobject offsets and value populations were inspected locally. FMRTE's proprietary executable/code and branding assets were not copied or bundled.
