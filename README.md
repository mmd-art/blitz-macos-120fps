# World of Tanks Blitz: 120 FPS on macOS

A one-line configuration workaround for the classic Steam client (DAVA). It uses the game's existing startup override mechanism. No mod loader, executable patch, repacked assets or downloaded binaries are needed.

```yaml
FPSLimit: "120"
```

## What was verified

On 10 October 2026, the Steam macOS client `11.20.133_7344899` accepted this override, logged and saved `FPSLimit: 120`. The player confirmed 120 FPS in the hangar. Sustained battle performance and frame-time measurements were not verified. Other Macs, the App Store edition and the Reforged/Unreal client were not tested.

The tested setup had `VSync: false` and a 120 Hz display. A 60 Hz display cannot show 120 distinct frames per second.

## Install

1. Quit Blitz normally.
2. In Finder, choose **Go → Go to Folder** and open:

   ```text
   ~/Library/Application Support/net.wargaming.wotblitz.macos/DAVAProject/
   ```

3. If `GraphicsOptionsOverrides.yaml` already exists, back it up and inspect it. Preserve existing settings; do not replace the file blindly. Add or update just the top-level `FPSLimit` setting.
4. Otherwise, copy this repository's `GraphicsOptionsOverrides.yaml` into that folder. Use the exact filename, without an extra `.txt` extension. The top-level setting has no enclosing `options:` section.
5. Launch through Steam and check the in-game FPS counter in the hangar and in a battle. If VSync is enabled, disable it in the graphics settings where available.

The graphics menu may still show only 30 and 60, with neither selected. That happened in the verified setup. Check the FPS counter for 120.

## Restarts and updates

In the inspected build, the game reads local options first, then the override, on every launch after quitting Blitz. Selecting 30 or 60 in the menu can change the current session; quitting and relaunching Blitz is expected to reapply 120.

The file is outside the game's application bundle. The inspected Steam game package did not contain it, and the inspected code for handling version updates did not remove it. Survival across a future game update has not been verified. A future client may change or stop supporting this mechanism.

## Undo

With Blitz closed, remove only the `FPSLimit` entry if the file has other overrides. If it contains only this workaround, rename it to `GraphicsOptionsOverrides.yaml.disabled`. Then launch and select 60 FPS in the graphics menu: the previous 120 value may remain in the local settings cache until changed.

## Background and related work

Static analysis of the tested build found that the game's macOS display setup reported a 60 FPS capability to the game even on the tested 120 Hz display. The existing override mechanism was sufficient to request the implemented 120 FPS mode without editing the app.

[An earlier public guide](https://github.com/nitlix/How-To-Get-120FPS-in-World-of-Tanks-Blitz-on-macOS/blob/main/guide.md) describes a different method for build `11.18.173_6747914`, involving executable and resource patches. Those patches were not used here.

This is a documented workaround, with no claim to being the first discovery. It is unofficial and has not been endorsed by Wargaming. This repository contains only the configuration example and original documentation, with no game files, account data or private diagnostic logs.

MIT-licensed repository contents. World of Tanks Blitz and its trademarks belong to their respective owners.
