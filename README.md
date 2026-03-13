# zmk-h3by3

ZMK firmware configuration for a **3×3 handwired macropad** powered by the [Nice Nano v2](https://nicekeyboards.com/nice-nano/) (nRF52840).

## Hardware

| Property  | Value |
|-----------|-------|
| Controller | Nice Nano v2 (nRF52840) |
| Layout | 3 rows × 3 columns (9 keys) |
| Wiring | Handwired matrix with diodes (col-to-row) |

### Pin assignment

| Function | nRF52840 pin | Nice Nano label |
|----------|-------------|-----------------|
| Row 0 | P1.13 | D13 |
| Row 1 | P1.11 | D11 |
| Row 2 | P0.10 | D10 |
| Col 0 | P0.11 | port 0, pin 11 |
| Col 1 | P1.04 | D4  |
| Col 2 | P1.06 | D6  |

### Physical layout

```
┌───┬───┬───┐
│ 1 │ 2 │ 3 │  Row 0 — P1.13
├───┼───┼───┤
│ 4 │ 5 │ 6 │  Row 1 — P1.11
├───┼───┼───┤
│ 7 │ 8 │ 9 │  Row 2 — P0.10
└───┴───┴───┘
 Col0 Col1 Col2
 P0.11 P1.04 P1.06
```

## Features

- **ZMK Studio** – real-time keymap editing over USB without reflashing firmware (`CONFIG_ZMK_STUDIO=y`)

## Building

This is a standard ZMK [user config repository](https://zmk.dev/docs/user-setup).  
The GitHub Actions workflow provided by ZMK will build the firmware automatically on every push.

To build locally:

```bash
west build -b nice_nano_v2 -- -DSHIELD=h3by3
```

The compiled firmware (`zmk.uf2`) will be in `build/zephyr/`.  
Put the Nice Nano v2 into bootloader mode (double-tap the reset button) and copy the file to the mounted drive.

## Keymap

The default keymap sends number keys **1–9**.  
You can customise it by editing `config/boards/shields/h3by3/h3by3.keymap` or use **ZMK Studio** at runtime.