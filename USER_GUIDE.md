# Glance Plex user guide

## Requirements and connection

Use Windows 10/11 (x64 compatible) with .NET Framework 4.8 or newer. Plex Media Server must be installed, running, and signed in under the **same Windows account** as Glance Plex. This version connects only to `http://127.0.0.1:32400`; remote servers, NAS devices, custom ports, and servers running under a different Windows account are not supported.

No token needs to be pasted. Glance Plex reads the current user's existing Plex token from the Plex Media Server registry settings at each refresh. It sends that token in a request header only to the local loopback server. It does not save the token, change server settings, stop streams, or collect playback history. Downloads contain no credentials, personal configuration, or real activity captures.

## Install or use portable

Download `Glance-Plex-v1.0.1-Setup.exe` from [the latest release](https://github.com/Brusko25/Glance-Plex-Releases/releases/latest). Run it and choose the optional desktop shortcut or Windows startup tasks if wanted. Installation is per-user and does not require administrator rights. The installer and application are unsigned, so Windows may display an unknown-publisher prompt.

Alternatively, extract every file in the Windows ZIP into a writable folder and run `GlancePlex.exe`.

The widget connects automatically on launch. If it cannot find Plex's saved sign-in, sign in to Plex Media Server as the current Windows user and right-click the widget → Refresh now.

## Read the widget

- **Nobody is watching:** Plex successfully reported zero active sessions.
- **Active streams:** includes playing and paused sessions; the smaller summary lists their states separately.
- **Activity unavailable:** the connection failed. Any rows shown are marked as last known and may have changed.
- Rows show the viewer, title, episode details where applicable, device, playback method when Plex provides it, and progress.
- Refresh defaults to 10 seconds. Paused playback remains visible. Failed connections retry after 30 seconds or the selected interval if longer.
- Scroll to see additional streams when the list fills the available screen height. Hover for full viewer, title, and device text.

## Controls

Drag to move. Right-click for Refresh now, Open Plex, Always on top, Lock position, refresh interval (5–60 seconds), opacity, Hide to tray, or Exit. Press F5 to refresh and Escape to hide. Double-click the tray icon or launch the app again to show it.

Position and display preferences are saved in `settings.json` beside the executable. Keep that file when updating.

## Update and uninstall

Exit the widget and run a newer installer, or replace the portable executable and documentation. Updates are manual. Reinstalling and uninstalling preserve your settings file. Uninstall from Windows Installed apps; the portable version can be removed by deleting its folder after exiting.

## Troubleshooting

If activity is unavailable, confirm that Plex Media Server is running, its local web interface opens at `http://127.0.0.1:32400/web`, and it is signed in under the same Windows account. Right-click → Refresh now. An authorization error means the saved local Plex sign-in was rejected; check Plex's own server sign-in. A missing widget may be hidden in the tray; launch it again to reveal it.

Report bugs at [GitHub Issues](https://github.com/Brusko25/Glance-Plex-Releases/issues). Never include Plex tokens or private viewer details in a public report.


## Checking for new versions

The app checks its public GitHub releases shortly after startup and daily. Choose **Check for updates** from the widget or tray menu to check immediately. When a newer stable version is available, you can open the release page. Downloads and installation remain manual; update checks send no account credentials or workspace data.
