# Changelog

All notable changes to Zelora Tune with AI are documented here. The project follows [Semantic Versioning](https://semver.org/).

## [1.5.0.1] - 2026-10-19

### Fixed

- **Filter bar overlap**: in a narrow column (e.g. the duplicate finder on a 1280-pixel-wide window) the search box was
  squeezed under the Filters button. The bar now puts the search box on its own row there and the controls below it.

## [1.5.0] - 2026-10-19

### Added

- **Filtering and sorting on every list the user chooses from**: Smart cleanup (all four tabs), duplicate groups, junk
  categories, registry issues, large files, startup entries, installed apps, leftovers, the force-uninstall review and
  backups. Depending on the list: multi-word search (accent-insensitive, "fotograf" finds "fotoğraf"), size range
  (KB/MB/GB), date presets or a custom range, chips (reason, risk, file type, hive, confidence, impact, location…),
  ticked / not ticked, sort field and direction. A counter shows "12 / 340 shown"; one click clears everything. Select
  all / none act on what is shown.
- **Smart cleanup criteria are now the user's**: installer, archive, partial-download, large-file and build-output
  ages and sizes, picture similarity, blur and screenshot thresholds — on the page and in Settings. Every forgotten
  file shows the rule it matched next to its own age ("unused for 212 days · rule: 90+ days in Downloads"). A
  Downloads folder moved to another drive or into OneDrive is recognized.
- **Full installation folder**: installing now lays out the complete application (ZeloraTune.exe, its assemblies and
  native libraries, translations, license texts) in Program Files instead of copying the single executable; updates
  replace it in place and remove only files an earlier version installed (`install.manifest`). The downloaded
  executable stays a single portable file. Release builds: `build/publish.ps1`.
- **Exit full screen**: Esc and F11 leave full screen (F11 also enters it), a button appears where the caption buttons
  were, and the window never comes back from the tray in full screen.

### Fixed

- **Registry cleaner removed entries the user had unticked** when they sat inside a ticked key (deleting a key deletes
  everything in it). Such keys are now kept and reported; unticking a category also unticks the issues already found
  in it, and the confirmation says how many ticked issues are not visible right now.
- **Startup manager "access denied"**: scheduled tasks are switched through the Task Scheduler API (schtasks re-registered
  the whole task and was refused for tasks running as another account); services whose Service Control Manager entry
  refuses changes get their start type set in the registry. Entries protected by their own software (e.g. antivirus
  self-protection) are marked "Protected" with an explanation instead of failing.

## [1.4.0] - 2026-10-19

### Added

- **Recycle Bin** in the junk cleaner: its size on every drive, emptied through the shell (like Explorer's "Empty
  Recycle Bin"), never file by file — the `$Recycle.Bin` folders stay protected for every other deletion. Not
  pre-selected: these are the user's own deletions.
- **Many more browsers**: Chrome Beta/Dev/Canary, Chromium, Edge Beta/Dev/Canary, Brave Beta/Nightly, Vivaldi,
  Yandex, Naver Whale, Cốc Cốc, Epic, Comodo Dragon, Cent Browser, Iridium, Slimjet, 360 Chrome, Thorium,
  Avast/AVG/CCleaner Secure Browser, Torch, Sidekick; Opera, Opera GX, Opera Air, Opera Beta/Developer; Firefox,
  Waterfox, LibreWolf, Floorp, Zen, Pale Moon, SeaMonkey, Mercury; and the Internet Explorer / legacy Edge cache.

### Fixed

- **Opera GX (and current Opera versions) were not detected**: they keep their profile in a `Default` sub-folder
  (and side-panel profiles in `_side_profiles`); only the old layout was recognized.
- Opera's code and GPU caches live in the roaming profile and were never cleaned; caches are now collected from both
  the roaming and the local profile folders, plus Opera's browser-wide `Cache` and `System Cache`.

## [1.3.0] - 2026-10-19

### Added

- **Registry Cleaner** page: finds registry entries left by removed programs that point at files, folders or programs
  that no longer exist — missing shared DLLs, obsolete "Apps & features" entries, App Paths, startup entries, Explorer's
  program name cache, Windows Installer folders, firewall rules, and (advanced, not pre-selected) file types, COM
  classes and services. Each entry shows the key's contents before it is fixed; fixing creates a restore point,
  backs everything up and can be undone from the Backups page. Only references that are certainly broken on a local
  fixed drive are reported (network/removable drives, access-denied and relative paths are never judged; 32-bit
  registry entries are resolved like a 32-bit program would; system components in the Windows folder are skipped).
- **Force uninstall wizard** for programs whose uninstaller is missing or fails: optionally runs the program's own
  uninstaller, then a deep analysis (registration, Windows Installer records, registry keys and values — App Paths,
  file types, context-menu commands, COM classes, Run entries, shared DLLs —, files, services, scheduled tasks,
  firewall rules, Store app data, running processes). A **registry review screen** lists every key and value with
  its contents; files and other items have their own tabs. Removal ends the processes, backs everything up and
  deletes it; locked files can be deleted at the next restart.

### Security

- **Registry protection policy**: hive roots, structural keys (Software, Classes, CLSID, Run, Uninstall, App Paths…)
  and sealed system trees (SYSTEM, SAM, SECURITY, Windows NT, Policies, Cryptography…) can never be deleted by any
  module, whatever the user's settings — in every spelling and registry view (HKCR, HKU\SID, WOW6432Node).

## [1.2.1] - 2026-10-19

### Fixed

- **Game mode** did almost nothing on many PCs: its default list only knew a few apps. It now also closes messengers
  (WhatsApp, Telegram, Signal…), Phone Link and nearby sharing, widgets, remote-access windows and more updater tray
  apps, and stops more background services (printing, link tracking, media sharing, updaters, diagnostics). Voice
  chat, browsers and gaming-gear software stay open.
- Store (MSIX) apps closed by game mode are now started again through the shell when it ends.
- Services are stopped and restarted in parallel, so switching game mode takes seconds instead of up to a minute; a
  service that is slow to stop is still restarted afterwards.
- The game mode switch no longer jumps back while it is working, and any error is shown on the card.
- Smart cleanup no longer offers screenshots of the same app as "similar photos" or as "blurry photos".

### Added

- Website screenshot set (`ZELORA_SCREENSHOTS=<folder> dotnet test`): every screen in Turkish and English and the
  dashboard in every theme, at 2× resolution, with fictional demo data.

## [1.2.0] - 2026-10-18

### Added

- **Smart cleanup** page, powered by on-device image analysis — nothing is uploaded, no model is downloaded:
  - **Similar photos:** bursts, resized and lightly edited copies are grouped (perceptual hash); the sharpest, largest
    shot is marked "Best" and always kept.
  - **Blurry photos** (Laplacian variance) and **screenshots** (name, folder, screen resolution, camera data,
    image structure), each with a confidence.
  - **Forgotten files:** old installers and archives in downloads, unfinished downloads, "(1)" copies, large unused
    files, build folders of projects untouched for months.
  - Nothing is marked automatically; "Mark suggestions" works per tab, removal asks first and always uses the
    Recycle Bin. Cleanups appear in the dashboard history.
- Smart cleanup is also in the tray menu and on the About page.

### Changed

- The application is now called **Zelora Tune with AI** (window, tray, setup, shortcuts, "Apps & features"). The
  executable, the program folder and the data folders keep their names, so an existing installation updates in place
  and keeps its settings; the old "Zelora Tune" shortcuts are replaced.

## [1.1.5] - 2026-10-18

### Added

- **Health score (0–100)** on the dashboard: free space, reclaimable junk, start-up load, boot time, drive health,
  security (antivirus, firewall, UAC, pending restart, update age), uptime and maintenance. Every lost point is listed
  with a **Fix** button that opens the right page.
- **Drive health (S.M.A.R.T.):** status, SSD life remaining, temperature, power-on hours and uncorrected errors.
- **Start-up & uptime:** last boot duration with a chart of recent boots, uptime and a restart hint.
- **Temperatures:** graphics card (NVIDIA, via the driver's NVML), drives and ACPI thermal zones — only sources Windows
  exposes without kernel drivers; missing sensors are stated plainly.
- **Network:** live download/upload rates with charts, connection type, IP address and link speed.
- **Hardware & system:** device, processor, graphics card(s) with VRAM and driver, memory (slots, speed), motherboard,
  BIOS, Windows version.
- **Security summary:** antivirus, firewall, UAC, Secure Boot, Windows Update, last restore point.
- **Cleanup summary:** total space reclaimed and recent cleanups.

### Fixed

- The junk cleaner now notices browsers and profiles installed while the app is running.

## [1.1.1] - 2026-10-18

### Added

- **Themes:** five designed color themes besides System/Light/Dark — Sunset Ocean (orange · ocean blue · gray),
  Forest, Neon Night, Sakura and Nordic — chosen from a swatch gallery in Settings and applied instantly.
- **Setup screen:** choose the language (the screen switches at once), the installation folder (default Program
  Files; other folders are locked down like Program Files) and the theme the app starts with, previewed live.
  `--install-dir=<folder>`, `--language=<culture|system>` and `--theme=<name>` for scripted use.
- **Tray menu:** opens every page and Settings directly, and is drawn by the app so it follows the selected theme.

### Changed

- Reopening the window from the tray (or starting the app again) always shows the dashboard.

### Fixed

- Choosing "system language" after another language now really switches back to the Windows display language.

## [1.1.0] - 2026-10-18

### Changed

- The application is now called **Zelora Tune** everywhere: `ZeloraTune.exe`, `%APPDATA%\ZeloraTune`,
  `%ProgramData%\ZeloraTune`, the `ZeloraTune` Task Scheduler folder and the source projects (`ZeloraTune.*`).
- **One executable for everything.** The first start of a downloaded `ZeloraTune.exe` asks whether to install it
  (Program Files, Start menu, "Apps & features", optional desktop shortcut) or run it portable (settings in
  `ZeloraTune.Data` next to the exe). Running a newer exe offers to update the installed copy. Scripted use:
  `--install [--desktop-shortcut]`, `--portable`, `--uninstall [--quiet]`. Translations and license texts are
  embedded; nothing ships next to the exe.
- **Game mode** moved to Settings as a one-click switch. Besides stopping non-essential services it now closes
  unimportant background apps (cloud sync, chat, updaters; editable list) and starts them again — with the user's
  normal rights and their original command line — when game mode is switched off, the app exits, or at the next
  launch after a crash. Work mode stays on the Speed & Startup page.
- Settings → About tab with the new logo, feature overview, version, installation type and license information.
- New logo and application icon.
- Startup checks the real requirement (Windows 10 2004+) and explains it in the UI language; the .NET runtime and
  graphics libraries are inside the executable, so nothing has to be downloaded.

### Security

The app always runs elevated, so anything a non-elevated process can change must never be trusted:

- Quarantine backups, the boost journal and temporary task definitions moved from the user profile to an admin-only
  folder (`%ProgramData%\ZeloraTune`, ownership verified at startup, fail-closed). Previously a non-elevated
  process could tamper with a backup and have it "restored" with admin rights, or get any service started.
- The single-file runtime no longer unpacks native libraries into `%TEMP%` (DLL planting): the executable
  restarts itself with the extraction folder inside the admin-only folder before any native code loads.
- Uninstall commands from per-user registrations (HKCU) and uninstallers stored in user-writable folders run with
  the user's normal rights instead of admin rights.
- Scheduled cleanup and launch at sign-in (which start the app elevated without a prompt) are only registered for
  an executable in an admin-protected location, i.e. the installed copy.
- Shredding refuses hard links and symbolic links, checked on the opened handle, so it can never overwrite the
  contents of another file.
- Task Scheduler XML is no longer written to `%TEMP%` between creation and import.

## [1.0.3] - 2026-10-18

### Added

- About page in Settings with the new logo.

## [1.0.1] - 2026-10-18

### Changed

- The application was renamed to **Sero Cleaner** (`SeroCleaner.exe`).
- Copyright holder: Sero Yazılım.
- New filled vector icons for Storage, Junk Cleaner, Duplicate Finder, Speed & Startup, Uninstaller and Settings
  (sidebar and dashboard), sharp at every DPI.
- On first launch the UI language follows the Windows display language (previously the OS install language);
  Turkish is used when the display language is not one of the five supported languages.
- Release packages are a single compressed `ZeloraTune.exe` without debug symbols or XML documentation.

## [1.0.0] - 2026-10-18

First public release for Windows 10 (2004+) and Windows 11, x64 and ARM64.

### Added

- **Dashboard:** live CPU, memory and disk gauges with history, drive overview, last cleanup,
  quick scan and boost actions.
- **Storage optimizer:** parallel folder analysis with treemap, folder table, large files and small-file clusters;
  SSD/HDD detection with TRIM or defragmentation and fragmentation analysis.
- **Junk cleaner:** system, browser (per profile), application and developer cache categories with risk levels;
  saved passwords and bookmarks protected by default; locked-file handling (skip, close owner, delete on reboot).
- **Duplicate finder:** three-stage hash pipeline (xxHash64 → xxHash128 / BLAKE3 / SHA-256), hard-link aware,
  smart selection rules, image preview; recycle bin, permanent, shred or move-to-folder removal.
- **Speed & startup:** startup manager for Run keys, Startup folders, scheduled tasks and services with impact
  estimates; memory booster; Game and Work modes with crash-safe service recovery.
- **Uninstaller:** desktop (Win32/MSI) and Store apps, batch and silent uninstall, force removal, leftover scan
  with confidence levels.
- **Safety engine:** quarantine vault with one-click restore and retention, automatic restore points before
  risky changes, protected system locations and user exclusions.
- **Backups & restore page:** quarantined backups and Windows restore points.
- **Automation:** scheduled and low-disk-space cleanups through Task Scheduler (`--scheduled-clean`,
  `--low-disk-check`), launch at sign-in, tray mode.
- **Settings:** eight sections with auto-save, import/export and reset.
- **Localization:** Turkish (primary), English, German, French and Italian, switchable live; user overrides from
  `%APPDATA%\ZeloraTune\Locales`.
- Apache License 2.0 with third-party notices shown in Settings → About.
