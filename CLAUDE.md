# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a ZMK keyboard firmware **user configuration** repository for a Corne split ergonomic keyboard. It does not contain ZMK firmware source code — it only contains keymap and hardware configuration files. The actual firmware is pulled from the official ZMK repository via the west manifest.

## Building Firmware

Firmware is built via **GitHub Actions** (the primary method). Push to any branch or open a PR to trigger a build. Download the compiled `.uf2` artifacts from the workflow run.

For local builds (requires a full ZMK development environment with `west`):
```sh
west build -b nice_nano_v2 -s app -- -DSHIELD="corne_left nice_view_adapter nice_view_custom"
west build -b nice_nano_v2 -s app -- -DSHIELD="corne_right nice_view_adapter nice_view_custom"
```

Flashing: double-press reset on the Nice!Nano to enter bootloader, then drag the `.uf2` file to the USB mass storage device.

## Key Files

- `config/corne.keymap` — All layer definitions, combos, and behaviors (ZMK devicetree syntax)
- `config/corne.conf` — Feature flags (display, pointing device, Bluetooth, etc.)
- `config/west.yml` — Dependency manifest: points to ZMK main branch and a custom `nice-view` module
- `build.yaml` — CI build matrix: two targets (left + right halves), both using `nice_nano_v2` board

## Keymap Architecture

The keymap has 6 layers:
1. **default** — QWERTY base; left control sends `Ctrl+Escape` on tap
2. **numbers** (LOWER) — Number row + navigation
3. **symbols** (RAISE) — Punctuation and symbols
4. **adjust** — Media, Bluetooth pairing (BT0–BT4), brightness; activates via conditional layer when LOWER+RAISE are both held
5. **system** — Bootloader access and layer toggles
6. **mouse** — Pointing device movement and scrolling

**Combos** (10 total, 30ms timeout): trigger bracket/paren/brace pairs and other symbols via simultaneous key presses. All combo definitions are at the top of `corne.keymap`.

## Hardware

- **MCU**: Nice!Nano v2 (nRF52840)
- **Display**: Nice! View (custom module via `west.yml`; gem animation disabled)
- **Pointing**: Enabled via `CONFIG_ZMK_POINTING=y`
- **Device name**: `smallboi` (set in `corne.conf`)

## External Tools

- Visual keymap editor: https://nickcoutsos.github.io/keymap-editor/ — uses `config/corne.json` for physical layout coordinates
- `my_keymap.png` — reference screenshot of the full keymap layout
