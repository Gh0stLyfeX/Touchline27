# Touchline 27 — Linux and Windows

Independent live editor for the supported Football Manager 27 build. Not affiliated with FMRTE, Sports Interactive or SEGA.

## Download and run

| System | Standalone download |
| --- | --- |
| Linux / CachyOS x86-64 | [Download Touchline27-CachyOS](https://github.com/Gh0stLyfeX/Touchline27/releases/latest/download/Touchline27-CachyOS) |
| Windows 10/11 x86-64 | [Download Touchline27-Windows.exe](https://github.com/Gh0stLyfeX/Touchline27/releases/latest/download/Touchline27-Windows.exe) |
| Source code for both editions | [Download source package](https://github.com/Gh0stLyfeX/Touchline27/releases/latest/download/Touchline27-GitHub-ready.zip) |

These are standalone programs: no Python installation or source-code setup is needed. Download the file for the computer where FM27 runs. The Windows executable does not run directly as a Linux application.

### Linux / CachyOS

1. Download **Touchline27-CachyOS** above and keep it in a convenient folder.
2. Right-click the downloaded file, open **Properties → Permissions**, and enable **Allow executing file as program** (wording depends on your file manager). Browsers may remove executable permissions during download.
3. Double-click the file. If your file manager asks what to do, choose **Execute**.
4. Alternatively, open a terminal in its folder and run `chmod +x Touchline27-CachyOS`, then `./Touchline27-CachyOS`.

No extraction, Python setup or FUSE is required on the tested CachyOS desktop. You may move or rename the executable. Keep it executable after copying it.

### Windows

1. Download **Touchline27-Windows.exe** above onto the Windows computer.
2. Put it wherever you want, for example your Desktop.
3. Double-click it. There is no installer and no Python requirement.
4. Run FM and Touchline under the same Windows account and privilege level. The executable is unsigned. If Windows displays a security warning, verify the source and checksum before deciding whether to run it; do not disable security protection globally.

The Windows edition has been tested under Wine; attaching to a real Windows FM27 career remains unverified.

## First connection

1. Start FM27 and load your career. Pause the game and create a separate save backup before editing.
2. Open Touchline and click **Load Game**. A running game may also be detected automatically.
3. Allow initial discovery to finish. It can take about a minute, depending on the career and computer. Subsequent connections to the same running game can use a validated cache.
4. Search a club, player or staff member by name or unique ID. Double-click the result to open its profile.
5. If you load a different career in FM, use **Rescan** before editing. A game restart requires discovery again.

Only the exact supported game module build enables named editing. A game update can therefore require a new Touchline layout/profile; do not force an unsupported build.

## Change a club's transfer or wage budget

1. Open **Clubs**, search the club and double-click it.
2. Open its finance page and find the remaining transfer budget, seasonal budget or weekly wage budget you want to change.
3. Enter the desired value and click **Save changes** or press **Ctrl+S**.
4. Review the before/after values in the preview, then apply the batch.
5. Return to FM, reopen the relevant screen if needed, and check the result. Save your career in FM to retain the live changes.

Some clubs have only a base balance object in the loaded career. Touchline does not invent budget fields when the full finance object is absent.

Money initially uses stored game units. If the displayed amount differs from FM, use **Currency calibration**, enter the exact raw amount from the field tooltip and corresponding amount shown in FM, then name your currency. Wages are weekly. Do not assume an uncalibrated number is pounds, euros or dollars.

## Edit players and staff

Use **Players** or **Staff**, search and open a profile, then edit the named pages. Player pages cover mapped attributes, ability, reputation and positions. Staff pages cover mapped coaching, recruitment, medical, personality and ability fields. Supported full contracts expose weekly wage and expiry date; basic and loan contracts remain read-only.

Review and apply changes using **Save changes**. Attribute values are generally shown on the 1–20 scale; current and potential ability use 1–200. The interface enforces the supported field ranges.

Use a club's **Squad** page to open its players and narrow the roster to the first team. Player/staff selections offer mass changes and reusable presets. Relative changes are clamped to valid ranges. Presets use Touchline JSON, not FMRTE preset files. Player comparison is read-only.

## Set team tactical knowledge to 100%

1. Open the club and select **Tactical knowledge**.
2. Choose **First team** or **All club squads**.
3. Select specific players if you want to limit the action; otherwise it targets the chosen squad.
4. Click **Full tactic knowledge (100%)**, review the batch, and apply.

This maximises **existing learned instruction and formation/role entries**. It does not create entries for an entirely untrained tactic, change the tactic, or guarantee FM's overall tactical-effectiveness gauge reaches 100%. Train a new tactic in FM first so records exist. Missing or invalid mapped training records cause the requested batch to be refused.

## Injury recovery tools

Squad/profile recovery actions are experimental: they set mapped injury light/full-training recovery durations to zero. They do not delete injury objects, restore condition, sharpness, fatigue or morale, or guarantee immediate injury removal. FM must process its medical update. Review the affected players and injuries before applying.

## Undo and save backups

Open **Changes**, select an applied batch and choose **Undo selected batch**. With no selection, **Ctrl+Z** targets the latest eligible batch. Undo only works while the current bytes and record identities still match the applied changes. FM may change values as time advances, so a separate save backup is the reliable recovery option.

The sidebar's **Back up a save…** copies a selected `.fm` file. Save/pause FM before copying. Live edits become durable only when you save in FM. UI undo covers the current attachment session; journals are not automatically replayed after restarting the editor or game.

## Settings, logs and troubleshooting

- **Linux:** settings and logs normally live in `~/.local/share/touchline27`, or `$XDG_DATA_HOME/touchline27`.
- **Windows:** settings and logs live in `%LOCALAPPDATA%\Touchline27`.
- **Won't start:** inspect `startup.log` in that folder. On Linux, confirm executable permission; launching from a terminal can show additional messages.
- **Game not found:** confirm FM27 runs on the same computer/account and a career is loaded, then select **Load Game**.
- **Access denied:** use the same privilege level as FM. On Linux, process-memory access restrictions may prevent attachment; the app does not change kernel security settings.
- **Unsupported build:** the installed game module does not match the supported fingerprint below. Stop named editing and use a matching supported version of the editor.
- **Value or ownership changed:** reload the record or rescan. Previewed bytes are checked again before writing, so stale changes can be refused.
- **After loading another career:** use **Rescan**.

## Compatibility and validation

Supported `game_plugin.dll` SHA-256:

```
bc940e472418fc5c214208d7cf30fecf600f689b6f9d71c11ba19a2df0fe057f
```

Read-only real game discovery was checked on CachyOS/Proton. Both backends passed the 28 controlled-process/UI tests, covering write/readback, undo, stale record rejection, rollback, contract dates, presets and tactical packing. The Windows standalone launch and Win32 process/module access were tested under Wine. Native Windows FM27 attachment and actual game behavior after applying edits still need verification in FM.

This is an experimental editor with a limited supported layout, not full FMRTE parity. Transfers/swaps, facilities, stadiums, competitions, freezing values, condition/sharpness restoration, and permanent injury removal are not implemented.

## Further documentation

- [Linux manual](MANUAL-LINUX.md)
- [Windows manual](MANUAL-WINDOWS.md)
- [All releases and downloads](https://github.com/Gh0stLyfeX/Touchline27/releases)

MIT licensed; see [LICENSE](LICENSE). Detailed manuals include research credits.
