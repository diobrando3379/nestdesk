# Changelog

All notable changes to this project are documented here. The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to
[Semantic Versioning](https://semver.org/).

## [3.2.0] - 2026-11-26

### Added
- **Gaps left by boxed icons are filled automatically** (on by default). Icons in a box still have a spot on the Windows desktop, so the icons left on the desktop used to show holes where boxed icons sat. Now the remaining desktop icons close up in their existing order (top to bottom, column by column), and the boxed icons' spots move after them, so the plain Windows desktop has no overlaps after quitting NestDesk. Only spots that already held an icon are reused; empty areas you left on purpose stay empty, and an icon you drag somewhere yourself stays there. Settings → 图标放进格子后，桌面上的空位自动补上.

## [3.1.1] - 2026-11-25

### Fixed
- The Recycle Bin icon now switches between full and empty like Explorer's. NestDesk watches the Recycle Bin itself, so emptying it (or deleting / restoring items) updates the icon right away; before, the icon could stay "full" after the bin was emptied.

## [3.1.0] - 2026-11-22

### Added
- **Arrange icons across or down**: each box can lay out its icons row by row (as before) or column by column, top to bottom like the Windows desktop. Box menu → 视图 → 横向排列 / 竖向排列.
- **Align icons left or right**: with 靠右 the first column is on the right edge of the box and the rest follow to the left (works with both directions). Box menu → 视图 → 靠左 / 靠右.

### Changed
- Boxes can now be made as narrow as one icon column (about 112 px at 100% scaling; a little wider with large icons). DeskGo's minimum width was 260. When a very narrow box is hovered, the title-bar buttons take the place of the title.
- **Automatic updates** (on by default): NestDesk checks the official release page in the background (shortly after start, then every 6 hours), downloads the new installer, verifies it against the published SHA256SUMS.txt, and installs it silently once you have been away from the computer for 5 minutes; it then starts again on its own. Settings → 自动更新 turns it off. The tray menu has **检查更新** to check and update right away. The portable version only shows a notice that links to the download page.

## [3.0.0] - 2026-11-22

Rewritten around the module structure of Tencent DeskGo 5.1.3.5 (desktop layer, box manager, data source, icon provider, clipboard, undo, menus). Behaviour, layout constants and colours follow DeskGo; the code is independent.

### Added
- **Frequently used box**: programs ranked by Windows' own UserAssist usage counts (top 60) plus Explorer's frequent folders; pin or remove items.
- **Recent documents box**: documents from the Recent folder, newest first; dropping a file onto it adds it to recent documents.
- **Notes box**: local sticky notes (plain text in the NestDesk data folder) edited in a small note window with auto-save.
- **Windows 11 style menus** for every menu (desktop, boxes, items, tray), including Explorer's shell context menu items and submenus; rounded acrylic panel, follows light/dark mode.
- **NestDesk entry in the system desktop right-click menu** (per-user registry verbs, no shell-extension DLL); can be turned off in Settings, removed on uninstall.
- **Undo delete (Ctrl+Z)**: deleted items are restored from the Recycle Bin, back into the box they were in. Shift+Delete stays permanent.
- **Flash and highlight**: items dropped, pasted or restored into a box flash blue (#2982FF, 3 pulses in ~1 s); search "locate" (Ctrl+Enter) selects and highlights an item that is on the desktop or in a box.

### Changed
- Box layout constants from DeskGo: 28 px title bar, 20 px title buttons with 4 px spacing, 10 px title margin, 260×300 default size, 260 px minimum width, grid items laid out from (4, 8) with 4/8 px gaps.
- Renaming an item uses an Explorer-style edit box: white, as wide as the item, centred, wrapping onto more lines as you type, with only the name (not the extension) selected.
- After renaming, keyboard focus returns to the desktop and the item stays selected.

## [2.0.1] - 2026-11-21

### Fixed
- Starting NestDesk no longer changes how the desktop looks. Icons outside boxes are drawn exactly where Windows has them (positions and spacing are read from the system desktop view), so nothing moves and no gaps appear at startup.
- Icon edges no longer look like a rough cut-out: shell icon bitmaps use straight alpha and are now premultiplied before drawing (icons were brighter at their anti-aliased edges).
- Icons, labels and overlays now match Explorer pixel for pixel at any scale: icon offset inside the item, icon-title font, 2-line label wrapping and line height, shortcut arrow size, and the UAC shield on programs that need administrator rights.
- Desktop icon hover/selection frames now match Windows exactly (measured from the system desktop): white at 35/255 on hover and 70/255 when selected, 1 px above the icon to just below the label, small rounded corners. Selection is hidden while the desktop doesn't have focus, and the focused item shows its full name over the icons below, like Explorer.
- Second line of a long name is shortened word-first ("Complete / Internet..."), like Explorer.
- Boxes behave like DeskGo: title-bar buttons appear only while the pointer is on the title bar; double-clicking the title renames the box (fold with the button); folded boxes expand while hovered (on by default); items use DeskGo's hover/selection fills; list rows are 26 px with 16 px icons.
- Box colours taken from DeskGo's own theme: translucent black (28%, 45% while active), 1 px white 10% border, 4 px corners, 12 px white title, DeskGo's rubber-band and snap-guide colours. Boxes still using the old default look are converted automatically.
- Boxes float over desktop icons: dropping or creating a box on top of icons no longer moves them (DeskGo behaviour).
- Box edge snapping (DeskGo BoxAlignerQt): while moving or resizing, an edge within 5 px of another box's edge aligns to it or docks next to it, with guide lines; boxes stay fully inside the work area.
- Rubber-band selection starts only after the drag threshold; a plain click on empty space clears the selection on release; Esc or right-click cancels a band and restores the previous selection.
- Cut items are drawn at 50% opacity until the clipboard changes.
- Type a letter (or pinyin initial) to jump to the matching item; repeat the letter to cycle.
- Dragging files over a folded box expands it.
- Desktop item menu: "放入新格子" puts the selected desktop items into a new box at the cursor.
- Slow second click on the name of a selected item starts renaming it (desktop and boxes), as in Explorer.
- Arranging, sorting and auto-arrange for desktop icons are handed to Windows itself, and icons you drag in NestDesk are written back to the system desktop, so quitting leaves the same layout.

## [2.0.0] - 2026-11-21

First public release.

### Added
- Boxes: move, resize, fold, lock, rename, color, opacity, icon/list view, sorting, manual drag order, rubber-band selection.
- One-click organize by editable rules (`rules.json`).
- Folder boxes that mirror a folder; network and removable paths load in the background and are re-checked every 30 s.
- Own desktop icon grid: per-monitor-layout cell positions, arrange by name/type/size/date, auto-arrange, overflow to secondary monitors.
- First launch keeps the icon positions you already had on the Windows desktop and does not organize anything automatically.
- Double-click an empty spot to hide/show everything.
- Desktop search (double <kbd>Ctrl</kbd> or a custom hotkey) with pinyin-initial matching and history.
- Native shell context menus, drag & drop with Explorer and other apps, cut/copy/paste, keyboard navigation.
- Optional frosted glass that falls back to translucency for live wallpapers, solid colors and slideshows.
- Layout backup/restore, pause, start with Windows (opt-in), `--restore` rescue switch.
- Per-user installer (Inno Setup) and offscreen benchmark mode (`--bench`).

### Performance
- Dirty-rect redraw for hover changes (~1.9 ms vs ~8 ms for a full 4K frame).
- Wallpaper for the frosted glass is decoded downscaled (≤ 1280 px), cutting its memory cost from ~+115 MB to ~+40 MB.
- A single long-lived COM worker thread replaces per-request threads (fixes slow kernel-handle growth).
- 1-hour steady-state runs show flat private memory, handles and GDI objects.

[3.2.0]: https://github.com/diobrando3379/nestdesk/releases/tag/v3.2.0
[3.1.1]: https://github.com/diobrando3379/nestdesk/releases/tag/v3.1.1
[3.1.0]: https://github.com/diobrando3379/nestdesk/releases/tag/v3.1.0
[3.0.0]: https://github.com/diobrando3379/nestdesk/releases/tag/v3.0.0
[2.0.0]: https://github.com/diobrando3379/nestdesk/releases/tag/v2.0.0
