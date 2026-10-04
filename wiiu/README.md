# 2 Ship 2 Harkinian 5.0.1 for Wii U

An unofficial Wii U port of [2 Ship 2 Harkinian](https://github.com/HarbourMasters/2ship2harkinian)
(2S2H), the PC port of The Legend of Zelda: Majora's Mask.

This release contains **no game data**. You supply your own Majora's Mask ROM and turn it into an
`mm.o2r` file once, on a PC, with the desktop version of 2S2H. The Wii U cannot do that step
itself: 2S2H's converter only exists in the PC builds.

## What is in the SD-card zip

Extract this zip to the root of the SD card. It mirrors the required SD-card layout:

| Path | What it is |
|---|---|
| `sd:/wiiu/apps/2s2h.wuhb` | The app you start from the Wii U Menu. |
| `sd:/wiiu/apps/2s2h/2ship.o2r` | 2S2H's own assets (fonts, menus, extra models). Not game data. |
| `sd:/wiiu/apps/2s2h/README.md` | This guide. |
| `sd:/wiiu/apps/2s2h/mods/` | Empty folder for optional texture packs. |

After extracting it, add only your generated `mm.o2r` to `sd:/wiiu/apps/2s2h/`.
Use the `2ship.o2r` from this zip, not the one from the desktop download: the Wii U build needs
its own copy. The bare `2s2h.rpx` is in a separate `-extras` zip for custom artwork; it is not part
of the SD-card zip.

## What you need

- A Wii U running the [Aroma](https://aroma.foryour.cafe/) homebrew environment.
- An SD card with about 100 MB free, plus room for any texture pack.
- A Windows, Linux or macOS computer, used once.
- A legally dumped Majora's Mask ROM that 2S2H supports: the **US N64 (NTSC-U 1.0)** or the
  **US GameCube** version. Check yours at [2ship.equipment](https://2ship.equipment/), or compare
  its SHA-1 with 2S2H's
  [list of supported hashes](https://github.com/HarbourMasters/2ship2harkinian/blob/develop/docs/supportedHashes.json).

Tested on a Wii U so far: the **NTSC-U 1.0** (US N64) ROM. The GameCube version goes through the
same code but has not been tried on a Wii U yet.

## Step 1: make `mm.o2r` on your computer

1. Download 2S2H **5.0.1** ("Battler Bravo") for your computer from the
   [2S2H releases page](https://github.com/HarbourMasters/2ship2harkinian/releases/tag/5.0.1).
2. Run it once with your ROM:
   - **Windows:** extract the zip, start `2ship.exe` and select your ROM when asked.
   - **Linux:** put the ROM in the same folder as `2ship.appimage`, then run it
     (`chmod +x 2ship.appimage` first if needed).
   - **macOS:** start `2ship.app` and select your ROM when asked.
3. Wait until the game's title screen appears, then close it. The new file is:
   - Windows and Linux: `mm.o2r` in the same folder as `2ship.exe` / `2ship.appimage`.
   - macOS: `~/Library/Application Support/com.2ship2harkinian.2s2h/mm.o2r`.

## Step 2: copy the files to the SD card

```
sd:/wiiu/apps/2s2h.wuhb
sd:/wiiu/apps/2s2h/2ship.o2r
sd:/wiiu/apps/2s2h/mm.o2r
```

The data folder must be called exactly `2s2h`, whatever you name the `.wuhb`. The app also keeps
its settings (`2ship2harkinian.json`) and save files there.

## Step 3: play

Put the card back, start the Wii U and open **2 Ship 2 Harkinian** from the Wii U Menu. The
first start takes a few seconds longer than later scene changes: the port loads `mm.o2r` into
memory at start so that moving between areas is quick.

## Texture packs (optional)

Texture packs are `.o2r` files. Put them in:

```
sd:/wiiu/apps/2s2h/mods/
```

Packs are made and hosted by their authors and are never bundled with this port. The pack tested
on the Wii U is GhostlyDark's [MM-Reloaded](https://github.com/GhostlyDark/MM-Reloaded), shrunk so
that no texture is larger than 256 pixels. The full HD download (about 3 GB) has not been tried
on a Wii U and is unlikely to fit in its memory. Pack textures show only while **Enable Mods** is
switched on in the in-game menu's Mods section.

## Troubleshooting

- **"Missing mm.o2r"**: the app cannot find `mm.o2r`. Hold the **POWER** button to turn off your
  Wii U, make `mm.o2r` on a PC (step 1), copy it into `sd:/wiiu/apps/2s2h/`, then start
  2 Ship 2 Harkinian again. Check that the name is exactly `mm.o2r`.
- **"Your mm.o2r was made with a different 2 Ship 2 Harkinian version"**: hold the **POWER**
  button to turn off your Wii U, make `mm.o2r` again with 2S2H 5.0.1 (step 1), copy it into
  `sd:/wiiu/apps/2s2h/`, then start 2 Ship 2 Harkinian again.
- **"2ship.o2r is missing or outdated"**: hold the **POWER** button to turn off your Wii U, copy
  `2ship.o2r` from this release zip into `sd:/wiiu/apps/2s2h/`, then start 2 Ship 2 Harkinian again.
- **Settings did not stick**: change them in the in-game menu. Don't edit `2ship2harkinian.json`
  while the game is running, because the game rewrites it when it closes.
- Always leave with **HOME -> Close** before taking the SD card out.

## Known limits

- Distant textures can shimmer (no mipmapping yet).
- Entering a new area can pause briefly while it loads from the SD card, more so with texture
  packs.

## Source code

This port is two branches on top of 2S2H's development branch shortly after 5.0.1 (commit
[`e8757c14a`](https://github.com/HarbourMasters/2ship2harkinian/commit/e8757c14a); it still reads `mm.o2r`
files made by 5.0.1):
[utsnik/2ship2harkinian `wiiu-release`](https://github.com/utsnik/2ship2harkinian/tree/wiiu-release)
and the GX2 (Wii U graphics) backend in
[utsnik/libultraship `2s2h-wiiu-release`](https://github.com/utsnik/libultraship/tree/2s2h-wiiu-release).

## Make your own artwork

The Wii U Menu icon is stored inside `2s2h.wuhb`. To use your own, build a new `.wuhb` from
`2s2h.rpx` (in the `-extras` zip) with `wuhbtool`, which comes with
[devkitPro](https://devkitpro.org/wiki/Getting_Started) (Wii U tools; on Linux and macOS
`sudo dkp-pacman -S wut-tools`). The icon is a 128 x 128 PNG; the optional boot screens are
1280 x 720 (TV) and 854 x 480 (GamePad). Put the files in one folder and run (one line):

```
wuhbtool 2s2h.rpx 2s2h.wuhb --name="2 Ship 2 Harkinian" --short-name="2S2H" --author="HarbourMasters" --icon=icon.png --tv-image=tv.png --drc-image=gamepad.png
```

Copy the new `2s2h.wuhb` to `sd:/wiiu/apps/`, replacing the old one. Your `2s2h` folder,
settings and saves stay as they are. If you share your `.wuhb` with others, don't put Nintendo's
logos or box art in it.

## Credits

- **[HarbourMasters](https://github.com/HarbourMasters/2ship2harkinian) and every 2 Ship 2
  Harkinian contributor.** This is their game; the port only adds the Wii U layer. If you enjoy it,
  support them.
- **[Kenix3 and the libultraship contributors](https://github.com/Kenix3/libultraship)**, plus the
  Fast3D authors.
- **[GaryOderNichts](https://github.com/GaryOderNichts)**, whose original Wii U port of Ship of
  Harkinian (the GX2 renderer and shader generator) this release builds on.
- **[GhostlyDark](https://github.com/GhostlyDark/MM-Reloaded)** for MM-Reloaded, the texture pack
  used for most of the Wii U testing. Download it from the author's page, not from a reupload.
- **devkitPro / wut** and the **[Aroma](https://aroma.foryour.cafe/)** team for the Wii U homebrew
  toolchain and environment.

This port is not affiliated with or endorsed by Nintendo, HarbourMasters or GhostlyDark.
