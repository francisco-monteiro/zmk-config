# ZMK Config — Lily58

Personal [ZMK](https://zmk.dev) firmware configuration for a **Lily58** split keyboard running on **nice!nano v2** controllers with **nice!view** displays.

## Hardware

| Component | Details |
|-----------|---------|
| Keyboard | Lily58 (split, 58 keys) |
| Controller | nice!nano v2 (both halves) |
| Display | nice!view (left) / nice!view custom (right) |
| Encoder | EC11 rotary encoder (right half) |
| Connectivity | Bluetooth (BLE) + USB |

## Bluetooth Connection

### How the split works

The two halves communicate with each other over BLE. The **left half is the central** — it connects to the host computer and relays keystrokes from the right half. The right half only talks to the left half; it never connects directly to the host.

> **Important:** Always plug USB-C into the **left half** when using a wired connection. Plugging into the right half will charge the battery but won't send keystrokes to the computer.

### First-time pairing (BLE)

1. Make sure both halves are powered on (slide the power switch on each nice!nano).
2. The two halves will pair with each other automatically — wait ~10 seconds.
3. On the **Raise layer** (hold right inner thumb key), the top row controls BLE profiles:

   | Key | Action |
   |-----|--------|
   | ESC | Clear current BLE profile (unpair) |
   | 1 | Switch to / pair on profile 0 |
   | 2 | Switch to / pair on profile 1 |
   | 3 | Switch to / pair on profile 2 |
   | 4 | Switch to / pair on profile 3 |
   | 5 | Switch to / pair on profile 4 |

4. Press one of the profile keys (e.g. `1` on Raise) to make the keyboard advertise on that profile.
5. On your computer, open Bluetooth settings and pair with **"Lily58"**.

### Switching between devices

You can store up to **5 BLE profiles** (profile 0–4). To switch to a different paired device, hold RAISE and press the corresponding number key. The keyboard will reconnect to whichever device was last paired on that profile.

### Troubleshooting BLE

| Symptom | Fix |
|---------|-----|
| Right half not sending keys | Left half is the central — check it's powered and paired to your computer |
| Plugged in USB but no input | Make sure USB is in the **left** half |
| Keys not registering after USB→BLE switch | Hold RAISE and press a profile key to force reconnection |
| Keyboard won't pair / connects but no input | Flash **settings reset** firmware to both halves (see Flashing below), then re-pair |
| Halves not talking to each other | Power-cycle both halves; if still failing, flash settings reset |

### USB vs Bluetooth priority

ZMK automatically prefers USB when a cable is connected to the left half. Unplug USB to fall back to BLE.

---

## Keymap

Three layers are active at all times. The two **inner thumb keys** activate layers:
- **Left inner thumb** (`LWR`) → hold for Lower layer
- **Right inner thumb** (`RSE`) → hold for Raise layer

### Layer 0 — Default

```
┌──────┬───┬───┬───┬───┬───┐                       ┌───┬───┬───┬───┬───┬───────┐
│ ESC  │ 1 │ 2 │ 3 │ 4 │ 5 │                      │ 6 │ 7 │ 8 │ 9 │ 0 │   `   │
├──────┼───┼───┼───┼───┼───┤                       ├───┼───┼───┼───┼───┼───────┤
│ TAB  │ Q │ W │ E │ R │ T │                      │ Y │ U │ I │ O │ P │   -   │
├──────┼───┼───┼───┼───┼───┤                       ├───┼───┼───┼───┼───┼───────┤
│LSHFT │ A │ S │ D │ F │ G │                      │ H │ J │ K │ L │ ; │   '   │
├──────┼───┼───┼───┼───┼───┼────────┐   ┌──────────┼───┼───┼───┼───┼───┼───────┤
│LCTRL │ Z │ X │ C │ V │ B │   [   │   │    ]     │ N │ M │ , │ . │ / │RSHFT  │
└──────┴───┴───┴─┬─┴─┬─┴─┬─┴─┬──────┘   └──────┬───┴─┬─┴─┬─┴─┬─┴───┴───┴──────┘
                 │GUI│ALT│LWR│  SPACE │   │ ENTER│ RSE │BSP│GUI│
                 └───┴───┴───┴────────┘   └──────┴─────┴───┴───┘
```

**Encoder (right half):** scroll left / right.

---

### Layer 1 — Lower (hold LWR)

Function keys, symbols, and media controls.

```
┌──────┬────┬────┬────┬────┬────┐                       ┌────┬────┬────┬────┬─────┬──────┐
│  F1  │ F2 │ F3 │ F4 │ F5│ F6 │                       │ F7 │ F8 │ F9 │F10 │ F11 │  F12 │
├──────┼────┼────┼────┼────┼────┤                       ├────┼────┼────┼────┼─────┼──────┤
│ CAPS │ ⏯  │ ⏮  │ ⏭ │    │    │                       │   │    │    │    │     │ PSCR │
├──────┼────┼────┼────┼────┼────┤                       ├────┼────┼────┼────┼─────┼──────┤
│  `   │ !  │ @  │ #  │ $ │ %  │                       │ ^  │ &  │ *  │ (  │  )  │  -   │
├──────┼────┼────┼────┼────┼────┼────────┐   ┌──────────┼────┼────┼────┼────┼─────┼──────┤
│      │EP+ │EP- │EP~ │   │    │        │   │          │    │ _  │ +  │ {  │  }  │  |   │
└──────┴────┴────┴─┬──┴─┬──┴─┬──┴─┬──────┘   └──────┬───┴─┬──┴─┬─┴───┴────┴─────┴──────┘
                   │    │    │ ▼  │        │   │      │     │    │   │
                   └────┴────┴────┴────────┘   └──────┴─────┴────┴───┘
```

- **EP+/EP-/EP~**: External power on / off / toggle (controls display backlight/power)
- **⏯ ⏮ ⏭**: Play/pause, previous track, next track
- **Row 3**: Shifted symbols `! @ # $ % ^ & * ( )`
- **Row 4 right**: `_ + { } |`
- **Encoder:** volume down / volume up

---

### Layer 2 — Raise (hold RSE)

Navigation, BLE profiles, and more symbols.

```
┌───────┬──────┬──────┬──────┬──────┬──────┐                       ┌────┬────┬────┬────┬──────┬─────┐
│BT CLR │ BT 0 │ BT 1 │ BT 2 │ BT 3 │ BT 4 │                      │    │    │    │    │ HOME │ END │
├───────┼──────┼──────┼──────┼──────┼──────┤                       ├────┼────┼────┼────┼──────┼─────┤
│   `   │  1   │  2   │  3   │  4  │  5   │                       │ 6  │ 7  │ 8  │ 9  │  0  │PGUP │
├───────┼──────┼──────┼──────┼──────┼──────┤                       ├────┼────┼────┼────┼──────┼─────┤
│  F13  │ F14  │ F15  │ F16 │ F17  │ F18  │                       │    │ ←  │ ↑  │ ↓ │  →   │PGDN │
├───────┼──────┼──────┼──────┼──────┼──────┼────────┐   ┌──────────┼────┼────┼────┼────┼──────┼─────┤
│  F19  │ F20  │ F21  │ F22  │ F23 │ F24  │        │  │          │ +  │ -  │ =  │ [  │  ]   │  \  │
└───────┴──────┴──────┴─┬────┴─┬────┴─┬────┴─┬──────┘   └──────┬───┴─┬──┴─┬─┴────┴────┴──────┴─────┘
                        │      │      │      │        │   │      │  ▼  │DEL │   │
                        └──────┴──────┴──────┴────────┘   └──────┴─────┴────┴───┘
```

- **Top row left**: BT CLR clears the current profile's pairing; BT 0–4 select/pair profiles
- **Row 2**: Number row duplicated (useful without leaving home position)
- **Row 3 left**: F13–F18 (extended function keys)
- **Row 3 right**: Arrow keys + Page Down
- **Row 4 left**: F19–F24
- **Row 4 right**: `+ - = [ ] \` and Delete
- **Encoder:** page up / page down

---

## Building Firmware

Firmware is built automatically via GitHub Actions on every push. Download the `.uf2` artifacts from the Actions tab.

Three build targets are defined in `build.yaml`:

| Target | Shield |
|--------|--------|
| Left half | `lily58_left nice_view_adapter nice_view` |
| Right half | `lily58_right nice_view_adapter nice_view_custom` |
| Settings reset | `settings_reset` |

## Flashing

1. Put the nice!nano into bootloader mode by double-tapping the reset button — it mounts as a USB drive.
2. Copy the corresponding `.uf2` file onto the drive.
3. Flash left half first, then right half.
4. Use the **settings reset** firmware to clear stored BLE bonds when pairing issues occur — flash to both halves, then re-flash the normal firmware.

## Configuration Highlights

- **NKRO** (N-Key Rollover) enabled over USB
- **BLE TX power** boosted to +8 dBm
- **Custom nice!view status screen** on the right half (via [aframires/nice-view-mod](https://github.com/aframires/nice-view-mod))
- BLE experimental features disabled for stability
