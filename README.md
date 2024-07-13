# Pocket openFPGA Atari System 1 Core with support for Analogizer-FPGA adapter
* Analogizer V1.0.0 [13/07/2024]: Added initial support for Analogizer adapter (RGBS, RGsB, YPbPr, Y/C, SVGA Scandoubler) and SNAC.
Adapted to Analogizer by [@RndMnkIII](https://github.com/RndMnkIII) based on **obsidian-dot-dev** openFPGA Atari System 1 Core:
https://github.com/obsidian-dot-dev/openFPGA-AtariSystem1

The core can output RGBS, RGsB, YPbPr, Y/C and SVGA scandoubler(0%, 25%, 50% 75% scanlines and HQ2x) video signals.

| Video output | Status |
| :----------- | :----: |
| RGBS         |  ✅    |
| RGsB         |  ✅    |
| YPbPr        |  ✅    |
| Y/C NTSC     |  ✅    |
| Y/C PAL      |  ✅    |
| Scandoubler  |  ✅    |

The Analogizer interface allow to mix game inputs from compatible SNAC gamepads supported by Analogizer (DB15 Neogeo, NES, SNES, PCEngine) with Analogue Pocket built-in controls or from Dock USB or wireless supported controllers (Analogue support).

**Analogizer** is responsible for generating the correct encoded Y/C signals from RGB and outputs to R,G pins of VGA port. Also redirects the CSync to VGA HSync pin.
The required external Y/C adapter that connects to VGA port is responsible for output Svideo o composite video signal using his internal electronics. Oficially
only the Mike Simone Y/C adapters (active) designs will be supported by Analogizer and will be the ones to use.

Support native PCEngine/TurboGrafx-16 2btn, 6 btn gamepads and 5 player multitap using SNAC adapter
and PC Engine cable harness (specific for Analogizer). Many thanks to [Mike Simone](https://github.com/MikeS11/MiSTerFPGA_YC_Encoder) for his great Y/C Encoder project.

For output Y/C and composite video you need to select in Pocket's Menu: `Analogizer Video Out > Y/C NTSC` or `Analogizer Video Out > Y/C NTSC,Pocket OFF`.
For output Scandoubler SVGA video you need to select in Pocket's Menu: `Analogizer Video Out > Scandoubler RGBHV`.

You will need to connect an active VGA to Y/C adapter to the VGA port (the 5V power is provided by VGA pin 9). I'll recomend one of these (active):
* [MiSTerAddons - Active Y/C Adapter](https://misteraddons.com/collections/parts/products/yc-active-encoder-board/)
* [MikeS11 Active VGA to Composite / S-Video](https://ultimatemister.com/product/mikes11-active-composite-svideo/)
* [Active VGA->Composite/S-Video adapter](https://antoniovillena.com/product/mikes1-vga-composite-adapter/)

Using another type of Y/C adapter not tested to be used with Analogizer will not receive official support.
============================================================================================================

Atari-System-1-compatible FPGA core for Analogue Pocket.

Based on the FPGA Atari System 1 core by d18c7db (Alex), ported from the [MiSTer version](https://github.com/MiSTer-devel/Arcade-Atari-system1_MiSTer).

## Compatibility

Supports all five Atari System 1 games that run on the original FPGA core:

* Indiana Jones and the Temple of Doom
* Marble Madness
* Peter Pack-Rat
* Road Runner
* RoadBlasters

## Features

### Service Mode

Toggling "Service mode" in the interact menu will reset the game to its service menu.  From here you can set the difficulty, health-per-coin, and other game parameters, as well as run through the various diagnostics tests.

Toggle "service mode" off to reset the game with the settings applied.  

Note: settings do not persist after exiting the core

### Control Options for Marble Madness

In handheld mode, the Marble is controlled with the dpad.

When docked and paired with a controller with analog controls, the left analog stick can be used to precisely control the Marble's speed and direction.

### Analog controls for RoadBlasters

When docked and paired with a controller with analog controls, the left analog stick can be used for precise steering, while the right analog stick can be used to control the throttle.

### Analog controls for Road Runner

When docked and paired with a controller with analog controls, the left analog stick can be used for movement. 

### Control Sensitivity

Control sensitivity can be selected for games featuring analog controls (Road Runner, RoadBlasters, and Marble Madness).  

Three sensitivity settings are available from the interact menu - low, medium, and high.  

- High-sensitivity controls allow for faster speeds and direction changes, at the expense of precision. 
- Low-sensitivity controls allow for greater precision of movement, at the expense of maximum speed.

## Usage

*No ROM files are included with this release.*  

Install the contents of the release to the root of the SD card.

Place the `.rom` files for the supported games onto the SD card under `Assets/system1/common`.

Note that generating the `.rom` format binaries used by this core is a bit more complex than usual.

First, you must run the associated per-game patching script to build intermediate zip files, containing assets from the base from the required ROMs corresponding to the files found in the most recent MAME release. These scripts pad the necessary roms to the required sizes, and place all dependencies in a single archive.

Then, these intermediate files can be processed using mra-tool with the provided `.mra` files to generate the correct `.rom` files accepted by the core.  

Note: these scripts have been tested on Linux; your mileage may vary on other targets.

Note: the `.mra` files provided are heaviliy modified from the original versions in MiSTeR due to the limitations of mra-tool (necessitating the rom pre-patching scripts). The original `.mra` files will not work for this core.

## History

v0.9.1
* Major refactoring to fix graphical bugs on newer pocekts.
* * Added SDRAM timing constraints.
* * Configured SDRAM I/O for fast input mode.
* * Switch to integer PLL structure, with dedicated PLL for SDRAM clock.
* * Integrated agg23's SDRAM controller.
* * Use SDRAM Controller's clock generation instead of driving separately via PLL.
* * Updated SDRAM CAS latency to CAS3 (datasheet says to use CAS3 beyond 85MHz)
* * Use BRAM for 64K of gfx rom, allowing us to avoid quad-word SDRAM reads. 
* Restore "Reset" and "Service Mode" interact options for games with analog controls.

v0.9.0
* Initial Release.

## License

All new materials for this port are licensed under the terms of the GPLv3.

Please see the headers and license files for the licensing terms of the individual dependencies.

## Attribution

```
Original Atari System-1 FPGA-compatible core by d18c7db (Alex)
https://github.com/d18c7db/atari_system1_fpga
---
# JT51
YM2151 clone in verilog. FPGA proven.
(c) Jose Tejada 2016. Twitter: @topapate
(https://github.com/jotego/jt51)
---
sdram-controller by agg23
https://github.com/agg23/sdram-controller
```