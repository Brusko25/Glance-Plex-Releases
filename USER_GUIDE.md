# Glance Plex user guide

## Install and connect

Use Windows 10/11 x64 compatible with .NET Framework 4.8 or newer. Download the **2.3.0 installer or Windows ZIP** from [the latest release](https://github.com/Brusko25/Glance-Plex-Releases/releases/latest). The per-user installer offers desktop and Windows startup shortcuts. The portable ZIP must be fully extracted into a writable folder. Builds are unsigned.

Plex Media Server on this PC, signed in under the same Windows account, connects automatically at `http://127.0.0.1:32400`. Right-click the widget → **Options → Plex connection** to change the address or port, supply a server-owner token for a different account/server, and test the connection. Automatic Windows sign-in is restricted to loopback addresses. Manual tokens are encrypted for the current Windows account. Glance never sends a token over plain http to an address outside your home network: use https for those servers. Local, private-network and Tailscale addresses (for example `192.168.x.x`, `10.x.x.x`, `100.64.x.x`–`100.127.x.x`, `.local`, `.lan` or `.ts.net` names) keep working over http.

## Protect playback with Tdarr

Tdarr integration is optional. Open **Options → Tdarr connection**, then **Test connection**. The default server API URL is `http://127.0.0.1:8266`; change it if needed and enter your Tdarr API key if authentication is enabled. The same home-network rule applies: an API key is sent over plain http only to local, private-network or Tailscale addresses.

The rule is simple: while anyone is watching Plex, playing or paused, any viewer, any device, Glance pauses **every** Tdarr node, including nodes that connect during playback. When no one is watching, it resumes them after the resume delay. **Pause Tdarr while someone is watching Plex** in **Playback** is on by default, and updating from 2.2.x turns it on once; you can turn it off there. With **Suspend current local encodes immediately** checked, Glance also suspends running FFmpeg/HandBrakeCLI processes descended from Windows Tdarr Node processes. It leaves Plex's own transcoder alone. Detection happens at the next Plex refresh (10 seconds by default), then the encoder guard checks every second. It includes Tdarr encoding health checks.

Immediate mode affects **all Tdarr encoders on this PC**. Run Glance under the same Windows account and privilege level as Tdarr; use **Check local encoder access** while encoding to verify access. Installation paths are discovered, so no personal folder is hardcoded. Tested against Windows Tdarr 2.89.01; other versions must expose compatible v2 node endpoints and Windows process names (`Tdarr_Node.exe` / `Tdarr_Node_Rust.exe`). Docker, WSL, remote encoders, and other process names cannot be suspended by this feature.

For a remote Tdarr server or inaccessible processes, uncheck immediate suspension. Queue-only mode lets current jobs finish and stops new jobs on every node. Remote Windows nodes can use queue control through the configured API, but their running encoders are not suspended by this PC.

When the last stream ends, the idle countdown defaults to **5 minutes**. Adjust it from 0 to 60 minutes. Playback returning resets the countdown. Losing Plex never counts as idle, but if Plex can't be reached while Tdarr is paused (for example the server is down or its sign-in expired), Glance releases its pause after **10 minutes** and notifies you. Change this under **If Plex can't be reached, resume after**: 10 minutes, 30 minutes, 1 hour or Never. When Plex comes back, protection resumes at the next refresh. Paused Plex sessions count as watching. Version 2.3.0 removed the older stream filters (transcoding only, remote only, a minimum stream count, ignored usernames) and node selection. Turn off automatic pause to leave encoding running normally while retaining status lights.

Queues already paused before Glance takes control remain paused. For queues Glance owns, it maintains its pause while anyone is watching and releases it after the delay. Avoid manually changing those same queues until Glance releases them. Closing Glance releases its local encoder suspensions and queue pauses. A separate helper also recovers local encoders if the widget exits unexpectedly or its heartbeat stops. If Tdarr is unavailable, queue recovery is saved and retried next launch; you can also unpause affected nodes in Tdarr. Do not delete recovery files while paused. Brief file-access conflicts retain the last trusted command; a missing heartbeat for 20 seconds triggers recovery. The helper exits after about a minute with no pause requested and restarts when needed. During Windows shutdown, cleanup is bounded and does not cancel the shutdown; unavailable queues retain their recovery lease.

## Read the widget

- Green CPU/GPU lights with **Working** indicate busy Tdarr transcode workers. Red with **Idle** means idle or confirmed suspended. Gray with **Unknown** means unavailable, suspension pending, or ambiguous remote status. Health-check workers are excluded from the counts.
- The percentages beside those labels show **overall usage on this PC**, including Plex and other apps, updated every two seconds. They do not measure only Tdarr or a remote node. CPU is total processor busy time; GPU is the busiest engine across local adapters, including video encoding, decoding, compute and 3D. An idle Tdarr worker can still have a nonzero hardware percentage.
- A dash (**—**) means the hardware reading is warming up, unavailable, or stale. GPU readings require working Windows GPU Engine performance counters and a compatible display driver; unsupported counters do not display a false zero. Monitoring uses Windows counters without vendor utilities or administrator prompts.
- Hover for counts, suspension status, and connection errors. Remote jobs may continue while local encoders are suspended.
- **Waiting for a paused node to reconnect** means a node Glance paused went offline; its queue is restored when it returns. **Encoder guard stopped unexpectedly · retrying in N s** means the recovery helper keeps exiting (for example its recovery file is unreadable); Glance retries after 2, 4, 8… up to 60 seconds.
- Active Plex rows show user, title/episode, device, playing/paused state, playback method, and progress. Scroll for more rows.
- **Nobody is watching** means Plex successfully returned no sessions. **Activity unavailable** means the last known rows may be stale; it is never treated as proof that playback stopped.

## Activity history

Open **Options → Activity** for searchable user, title/episode, device, first/last observed time, observed playing duration, and state. Recording is on by default, retained for 90 days; choose 1–365 days, turn recording off, or clear it. Up to 10,000 records are retained and the newest 1,000 matching records are displayed.

History is recorded locally only while Glance runs. Playing duration is an estimate from consecutive successful polls, excludes observed pauses, and does not count connection gaps, stopped-app time, or seeking as extra watching. Short sessions between polls can be missed. It is not a backfill of Plex's historical activity. Session and state changes save immediately; routine playing progress and last-seen timestamps save at most once a minute and are flushed when Glance closes. A sudden power loss can still lose unsaved progress.

## Options and controls

Right-click → **Options** organizes playback protection, Tdarr connection, Plex connection, Activity, and Widget preferences. The resizable window uses dark cards, toggle switches, and a searchable history table. Monitoring continues while it is open; Save changes applies your settings. Drag the widget to move, F5 refreshes, Escape hides, and double-clicking the tray icon reveals it. Choose opacity, always on top, position lock, and a 5–60 second refresh interval. Failed Plex requests retry after at least 30 seconds.

## Updates and uninstall

Glance checks public GitHub releases after startup and daily. Choose **Check for updates** from the widget/tray menu, or the **Check for updates** button at the top of Options. A newer version offers **Update now**. It downloads the matching installer for installed copies or ZIP for portable copies, verifies SHA-256 and the product/version, releases Glance's Tdarr pauses, saves preferences, closes, installs, and reopens. Updates require your click. If preparation fails, the app stays open. Download failures do not replace files; a failed portable replacement is rolled back.

Settings, credentials, and history are not package targets. Reinstalling or uninstalling also preserves local data. You can still exit Glance and run a newer installer manually. Version 1.0.1 requires one manual upgrade to get the new updater. Changing from portable to installer uses a different folder; retain/copy your settings deliberately after exiting. Encrypted credentials cannot be moved to another Windows account.

## Local files and privacy

Files beside the executable: `settings.json` (preferences), `plex-connection.json`, `tdarr-settings.json` (encrypted manual credentials), `activity-history.json` (plain-text viewing records), `tdarr-lease.json`, and `tdarr-guard-*.json` (recovery). Keep them during upgrades, never distribute your running app folder. Use the clean release installer/ZIP when sharing.

Automatic Plex tokens are read from the Windows account's Plex registry settings at request time and are not saved. Tokens are sent only to the configured Plex endpoint; Tdarr receives its own API key. GitHub update checks/downloads receive no Plex activity, credentials, or settings. No viewing history is uploaded.

If a test fails, verify the server is running, address/port and API key/token are correct, and Glance has matching Windows privileges. A zero encoder-access result while idle is inconclusive: start an encode and test again. Report issues through [GitHub Issues](https://github.com/Brusko25/Glance-Plex-Releases/issues); omit credentials and private viewer details.

## Snap to other Glance widgets

In **Options → Widget**, **Snap to Glance widgets** aligns nearby widget edges while dragging. It starts enabled, including after upgrading older settings. Turn it off for free dragging. Pull farther than 12 logical pixels from an edge to release it; release the mouse to save the final position.

Both apps need shared snapping protocol v1. Older unmarked Glance versions do not participate. Locked widgets remain anchors; a widget with snapping off can still be an anchor for another app. Hidden, minimized, and other-desktop windows are excluded. Only the widget you drag moves. Options, previews, startup, and activity refreshes do not cause snapping.
