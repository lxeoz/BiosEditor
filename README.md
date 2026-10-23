# BiosEditor

A Windows desktop app for viewing and editing **AMI BIOS settings** through the `nvram.txt` dumps produced by AMI's SCEWIN / SCEHUB tools.

Instead of scrolling through a dump file with thousands of entries in a text editor, BiosEditor shows every setting in a searchable list. You pick options from a menu, see what you changed, and can flash the result back to the board.

> **Warning:** changing BIOS settings can make a system unstable or unbootable. Back up your current settings (Export, then **Save copy as…**) before importing anything, and only change settings you understand.

## What it does

- **Export from BIOS**: runs SCEWIN (with a UAC prompt) to read the board's current settings into `nvram.txt` and loads it.
- **Edit**: search by name, help text or value; filter by option settings or value settings; pick options from a list or type values, with format and range checks.
- **Import to BIOS**: preview every change first, then write the edited `nvram.txt` back to the board.
- **vs Defaults**: lists every setting that differs from its BIOS default (default → current). It works on any dump, so it shows settings changed in the BIOS menu or by other tools, not just edits made in the app. Each one can be reset with **Use default**.
- **Compare**: diffs two dumps setting by setting (changed / only in A / only in B), including settings that moved between BIOS versions. Results can be copied as presets or as a text report.
- **Presets**: paste lines like `SVM Mode = Enabled` or `Global C-state Control [Disabled]` to apply many settings at once.
- **History**: a log of every change in the session, with a jump to each setting.
- **Unsaved-changes protection**: asks before discarding edits, and **Revert** puts any setting (or all of them) back to the loaded value.

## Requirements

- Windows 10 / 11 (64-bit); Export / Import need admin rights (the app asks through UAC)
- An AMI-based motherboard
- **SCEWIN is not included.** Put the SCEWIN folder (`SCEWIN_64.exe` and its drivers) next to the BiosEditor exe. Without it the app can still open, edit, compare and save `nvram.txt` files, but cannot Export or Import.

## Typical use

1. Click **Export from BIOS** to load the board's current settings.
2. Look at **vs Defaults** to see what is already off stock.
3. Change what you need in the **Editor** (changed settings turn yellow).
4. **Save nvram.txt**, then **Import to BIOS**, review the preview, and confirm.
5. Reboot for the changes to take effect.

## Shortcuts

`Ctrl+O` open · `Ctrl+S` save · `Ctrl+F` search · arrow keys / PageUp / PageDown / Home / End to move through lists.

Files can also be dragged onto the window, or passed on the command line: `bios_editor.exe a.txt` opens a file, `bios_editor.exe a.txt b.txt` opens two files in Compare.

## Troubleshooting

Every run writes `bios_editor_log.txt` next to the exe (or in `%TEMP%`). If the window doesn't open, try `--renderer wgpu` or `--renderer glow`, or run with `--debug` for a detailed log.

## Built with

Rust and [egui](https://github.com/emilk/egui).
