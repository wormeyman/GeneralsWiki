Tested on an M4 Pro MacBook Pro (24 GB RAM) running macOS Tahoe 26.4. The game runs through Wine-CrossOver via Heroic Launcher, no native macOS build exists yet. Performance and audio were both fine.

## What you need

- A Mac with Apple Silicon (M1 or later)
- macOS Tahoe (or recent macOS — older versions should work but aren't tested here)
- [Heroic Games Launcher](https://heroicgameslauncher.com/) (tested with 2.20.1)
- A Steam account that owns *Command & Conquer: Generals - Zero Hour* (app ID `2732960`)
- [Homebrew](https://brew.sh/) installed

## Step 1: Install SteamCMD

```
brew install steamcmd
```

## Step 2: Download the game files

Run this command. Replace `YOUR_USERNAME` with your Steam username.

```
steamcmd +@sSteamCmdForcePlatformType windows +login YOUR_USERNAME +app_update 2732960 validate +quit
```

The `+@sSteamCmdForcePlatformType windows` flag forces Steam to download the Windows version of the game, since there's no macOS build.

Steam Guard will ask you to confirm on your phone. The download is about 3 GB.

Files end up in:

```
~/Library/Application Support/Steam/steamapps/common/Command & Conquer Generals - Zero Hour/
```

## Step 3: Install the community patch

Go to the [GeneralsGameCode releases page](https://github.com/TheSuperHackers/GeneralsGameCode/releases) and download the latest `weekly-*-generalszh` build.

Extract the files directly into the game folder from Step 2, overwriting the original executables.

## Step 4: Install Wine-CrossOver in Heroic

Open Heroic Games Launcher. Go to **Wine Manager** (in the sidebar or settings) and install **Wine-CrossOver-Latest**. Version 23.7.1-1 was used here.

> ⚠️ **A note on GPTK**: Game Porting Toolkit was also tested but crashed on the splash screen. Wine-CrossOver worked without issues. If a future GPTK version fixes this, it may be worth retrying since Apple's DirectX-to-Metal translation is theoretically a better long-term path.
> 

## Step 5: Add the game to Heroic

1. Open Heroic and go to **Library**
2. Click **Add Game**
3. Fill in:
    - **Name**: Generals Zero Hour (or whatever you want)
    - **Platform**: Windows
    - **Executable**: browse to `generalsZH.exe` in the game folder
4. Make sure **Wine-CrossOver** is selected as the Wine version (not GPTK) you may need to do this step later.

Heroic creates the Wine prefix automatically. No need to configure it manually.

## Step 6: Launch and play

Hit play. No Winetricks dependencies were needed, it ran out of the box with Wine-CrossOver.

Set your resolution in the game's graphics options. 1900x1200 worked on a 14" MacBook Pro display.

## Known issues

### Mouse gets stuck on first launch

The mouse cursor can get trapped or stop responding after the game launches. Pressing **Cmd+Tab** to switch away from the game and then back a few times fixed it. It's not clear whether this happens every launch or just the first time.

If Cmd+Tab doesn't fix it, try enabling **Virtual Desktop** in Heroic's Wine configuration settings. That gives Wine its own contained window and usually resolves mouse capture problems with old DirectX games.

## What was NOT tested

- Online multiplayer
- The base *Generals* game (only *Zero Hour* was tested)
- Intel Macs
- Game Porting Toolkit (crashed — see note above)
- Mods

## Troubleshooting

Heroic logs are at:

```
~/Library/Application Support/heroic/GamesConfig/last.log
```

Check there if the game won't start. Common things to look for: D3D errors, missing DLL calls, or Wine prefix initialization failures.
