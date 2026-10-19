# Zelora Tune with AI

A native PC cleanup and system optimizer for Windows (x64 / ARM64), built with .NET 10 and Avalonia UI.
The core engine is platform-independent, so Linux and macOS support can be added later.

## Features

* **Dashboard:** health score with one-click fixes, live CPU/memory/disk and network charts, drive health
  (S.M.A.R.T.), start-up time and uptime, temperatures, hardware summary, security status and cleanup history.

* **Storage optimizer:** treemap and folder table of what fills each drive, large files, clusters of tiny files.
  The right maintenance per drive: TRIM for SSDs, defragmentation only for HDDs.
* **Junk cleaner:** temp files, caches, crash dumps, update leftovers, the Recycle Bin and per-profile browser data for
  some 40 browsers (Chrome, Edge, Brave, Vivaldi, Opera / Opera GX, Firefox and their forks…).
  Saved passwords and bookmarks are protected by default.
* **Duplicate finder:** size → partial xxHash64 → full xxHash128 / BLAKE3 / SHA-256, smart selection rules,
  image preview. One copy of every group is always kept.
* **Smart cleanup (AI, on-device):** finds similar photos (bursts, resized or edited copies) and suggests the best
  shot to keep, blurry photos, screenshots and forgotten files (old installers, unfinished downloads, "(1)" copies,
  old archives, stale build folders). Nothing leaves the PC and nothing is deleted automatically; removal goes to
  the Recycle Bin after confirmation.
* **Speed & startup:** startup manager with impact estimates, one-click memory boost, work mode.
* **Game mode:** one click in Settings closes unimportant background apps and stops non-essential services;
  everything starts again when it is switched off.
* **Registry cleaner:** invalid entries of removed programs (missing DLLs, obsolete uninstall entries, App Paths,
  startup entries, firewall rules, file types, COM classes…), each shown with its key contents, backed up before it is
  fixed. System keys are protected by a hard-coded policy.
* **Uninstaller:** batch uninstall of desktop and Store apps and a leftover scan; a step-by-step **force uninstall**
  wizard for programs whose uninstaller is missing or broken, with a review screen for every registry key it removes.
* **Backups & restore:** everything Zelora Tune with AI removes is quarantined and can be restored with one click;
  Windows restore points are created before risky changes.
* **Automation:** scheduled and low-disk-space cleanups through Task Scheduler, silent, without a UAC prompt.
* Five languages, eight themes (System, Light, Dark, Sunset Ocean, Forest, Neon Night, Sakura, Nordic),
  Mica/Acrylic glass, tray mode with a themed menu.

## Install or run portable

Download `ZeloraTune.exe` and start it. The first start asks whether to **install** it (into the folder you choose,
default Program Files; Start menu, "Apps & features") or **run it portable** (settings in `ZeloraTune.Data` next to
the exe, e.g. on a USB drive), in which language and with which theme.
Nothing else needs to be installed: the .NET runtime and graphics libraries are inside the executable.
Uninstall through "Apps & features".

Command line: `ZeloraTune.exe --install [--install-dir=<folder>] [--desktop-shortcut]`, `--portable`,
`--uninstall [--quiet]`, `--theme=<SunsetOcean|Forest|NeonNight|Sakura|Nordic|Light|Dark|System>`,
`--language=<tr-TR|en-US|de-DE|fr-FR|it-IT|system>`.

## Build & run

Requires the .NET 10 SDK.

```bash
dotnet build ZeloraTune.slnx
```

```bash
dotnet test ZeloraTune.slnx
```

```bash
dotnet run --project src/ZeloraTune.App
```

The executable requests administrator rights (UAC) because disk, service and registry operations need them.
Windows 10 version 2004 (build 19041) or later is required.

## Release build

```powershell
./build/publish.ps1 -Output artifacts
```

For each runtime (win-x64, win-arm64) the script publishes the install layout (the multi-file folder an installation
lays out in Program Files), zips it, and embeds it in one compressed, self-contained `ZeloraTune.exe` — portable app,
installer and uninstaller in a single file. Installing unpacks the full folder; running portable needs nothing else. See [CHANGELOG.md](CHANGELOG.md) for release notes.

## Languages

**Türkçe** (primary), English, Deutsch, Français, Italiano. Turkish is the master dictionary and the fallback for
any untranslated text. Translations are plain JSON in [`Locales/`](Locales/). To fix or extend a
translation without rebuilding, drop a partial `{culture}.json` into `%APPDATA%\ZeloraTune\Locales`
(portable: `ZeloraTune.Data\Locales`).

## Security

Zelora Tune with AI runs with administrator rights, so it never trusts anything a non-elevated process could change:
backups, journals and its own runtime files live in an admin-only folder, user-planted commands run with normal
rights, and tasks that start it elevated exist only for the installed copy. Details in
[ARCHITECTURE.md](ARCHITECTURE.md#security-model).

## Architecture

See [ARCHITECTURE.md](ARCHITECTURE.md).

## License

Copyright 2026 Sero Yazılım

Licensed under the [Apache License, Version 2.0](LICENSE). See [NOTICE](NOTICE) and
[THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt) for the licenses of bundled third-party components.
