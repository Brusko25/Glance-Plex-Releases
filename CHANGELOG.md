# Changelog

## 2.2.3 — 2026-09-25

- If Plex can't be reached while Tdarr is paused (server down or sign-in expired), Tdarr resumes after 10 minutes and you get a notification. Choose 30 minutes, 1 hour or Never in Options → Playback. Before, Tdarr stayed paused until Plex came back.
- Your Plex token and Tdarr API key are never sent over plain http to an address outside your home network. Use https for those servers; local, private and Tailscale addresses keep working over http.
- When a paused Tdarr node disconnects, the status says Glance is waiting for it to reconnect, instead of reporting a connection problem.
- If the encoder guard keeps stopping (for example because its recovery file is unreadable), Glance retries after 2, 4, 8… up to 60 seconds instead of every second, and says so.
- The pause rule is evaluated once per refresh, and the widget reuses its fonts instead of creating new ones on every repaint.
- Viewing history shows "Sep 21 8:00 PM" instead of running the date and time together ("Sep 218:00 PM").
- Saves to the same file from two threads now take turns. They could fail after five quick retries on Windows. This showed up as an intermittent test failure, and Glance Platform had already worked around it in its Plex tile.

## 2.2.2 — 2026-09-24

- Fix a crash while exiting: widget resource cleanup is safe when Windows Forms and the application scope both dispose the widget.
- Handle an already-cleared tray menu and dispose the owned icon only once.
- Add non-preview regression checks for repeated disposal and a missing tray menu.

## 2.2.1 — 2026-09-23

- Keep the last trusted playback command during brief file-access failures, preventing those failures from resuming encoders and unpausing the Tdarr queue mid-playback. Allow brief startup file contention and retry pending recovery-journal saves.
- Let the encoder guard exit after a minute with nothing paused, instead of running and rewriting its files every second until the app closes.
- Save viewing history only when something changes, and playing progress at most once a minute, instead of rewriting the whole history file on every refresh.
- Don't block Windows shutdown or restart; do a short cleanup instead.
- Use a separate temporary file for each save and write settings, lease and history files through to disk before replacing them; handle unreadable Tdarr settings and a failed "Open Plex" gracefully.

## 2.2.0 — 2026-09-21

- Show live local CPU/GPU percentages beside Tdarr worker status, sampled off the UI thread every two seconds.

- Show Working beside green CPU/GPU indicators and Idle beside red indicators; unconfirmed status stays Unknown.

- Add the app icon and Glance Plex name to the widget header, remove the redundant idle subtitle, and enlarge the Tdarr CPU/GPU indicators.

## 2.1.0 — 2026-09-21

- Add shared Glance widget snapping during dragging, with a default-on Widget option and cross-process discovery. Other apps need protocol v1 support.

- Match Glance Finance’s thin gray widget border and rounded corners.


## 2.0.0 — 2026-09-21

- Promote the approved Finance-style interface to the Glance Plex 2.0 release.
- Includes Tdarr playback protection, adjustable resume timing, CPU/GPU indicators, configurable connections, local activity history, and verified in-app updates.
- Match Finance’s header and navigation spacing, with a top-right Check for updates button that protects unsaved option edits.
- Remove redundant page introductions so settings cards sit directly beneath navigation.
- Existing settings, history, and playback behavior are preserved.


## 1.1.1 — 2026-09-21

- Redesign Options to match Glance Finance: navy panels, amber accents, rounded cards, and clear navigation.
- Replace white inputs with dark fields and toggle switches; keep Save changes and Cancel visible.
- Add a compact activity toolbar, dark history table, and resizable two-column settings pages.
- Support opening Options directly with `GlancePlex.exe --options`.


## 1.1.0 — 2026-09-21

- Optional immediate suspension of local Windows Tdarr encoders during Plex playback, with a separate recovery helper. Queue-only mode and node selection are also available.
- CPU/GPU status lights and an adjustable idle delay, defaulting to five minutes.
- Options window for playback filters, connection tests, encrypted manual credentials, widget appearance, and activity history.
- Local viewing history with observed playing time, search, retention, recording toggle, and clear control.
- In-app verified updates for installed and portable copies; preserve settings/history and release Glance-owned Tdarr pauses before replacement.


## 1.0.1 — 2026-09-15

- Detect newer stable releases at startup and daily; add manual update checks and optional links to the product download page.

- Guard live activity captures with a required live- filename, block publication image folders, and label them LIVE · DO NOT PUBLISH.
- Preserve Plex XML parsing errors with their original cause for troubleshooting.

## 1.0.0

- First release of Glance Plex for Windows 10/11 with .NET Framework 4.8.
- Automatic connection to Plex Media Server on the same PC and Windows account.
- Active stream count, usernames, titles, playing/paused state, devices, progress, and playback method when provided by Plex.
- Explicit offline and last-known-activity states; automatic retry after failures.
- Draggable widget, tray controls, adjustable refresh interval, opacity, always on top, and position lock.
- Original amber activity icon embedded in the app and installer.
- Per-user installer with optional desktop shortcut and Windows startup, plus portable ZIP.

Version 1.0.0 required manual update installation.
