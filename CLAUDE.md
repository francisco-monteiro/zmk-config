# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a ZMK firmware configuration for a **Lily58 split keyboard** with nice!nano v2 controllers and nice!view displays. Firmware is built via GitHub Actions — there is no local build toolchain in this repo.

## How Firmware is Built

Firmware is built automatically by the ZMK GitHub Actions workflow (`.github/workflows/build.yml`), which delegates to the upstream `zmkfirmware/zmk` reusable workflow. To trigger a build, push to `master` or open a pull request. Artifacts (`.uf2` files) are produced for each target in `build.yaml`.

There is no local `west build` or `cmake` setup here — flashing requires downloading the build artifacts from GitHub Actions.

## Repository Structure

- **`build.yaml`** — defines the three build targets: left half, right half, and settings reset. Each target uses `board: nice_nano//zmk` at revision v2.
- **`config/lily58.keymap`** — the keymap in Devicetree/ZMK syntax. Three layers: `default_layer`, `lower_layer` (LOWER held), `raise_layer` (RAISE held).
- **`config/lily58.conf`** — Kconfig options enabling NKRO, nice!view display with custom status screen, EC11 encoder, and BLE TX power boost.
- **`config/west.yml`** — West manifest pulling ZMK from `zmkfirmware/zmk@main` and `aframires/nice-view-mod@main` for the custom right-side nice!view display.
- **`boards/shields/lily58/lily58.dtsi`** — custom shield overlay adding right-side encoder (EC11 on pro_micro pins 20/21) and OLED (SSD1306 at I2C 0x3c).
- **`zephyr/module.yml`** — marks this repo as a Zephyr module named `zmk-config`.

## Key Configuration Details

- **Display**: `CONFIG_ZMK_DISPLAY_STATUS_SCREEN_CUSTOM=y` — the right half uses `nice_view_custom` shield from the `nice-view-mod` module (not the stock nice!view).
- **NKRO**: Enabled via `CONFIG_ZMK_HID_REPORT_TYPE_NKRO=y` and `CONFIG_ZMK_HID_KEYBOARD_NKRO_EXTENDED_REPORT=y`.
- **BLE experimental features**: explicitly disabled (`CONFIG_ZMK_BLE_EXPERIMENTAL_FEATURES=n`).
- **Encoder**: Right half only; default layer scrolls left/right, lower layer controls volume, raise layer scrolls page up/down.
- **Sleep/RGB**: commented out in `lily58.conf` — preserved for easy re-enabling.

## Making Keymap Changes

Edit `config/lily58.keymap` using ZMK's Devicetree binding syntax. Key references:
- `&kp KEY` — keypress
- `&mo N` — momentary layer N
- `&bt BT_*` — Bluetooth actions
- `&ext_power EP_*` — external power control
- `&trans` — transparent (pass through to lower layer)

Layer order matters: layer 0 = default, layer 1 = lower, layer 2 = raise. The thumb cluster uses `&mo 1` (left inner) and `&mo 2` (right inner) to activate layers.
