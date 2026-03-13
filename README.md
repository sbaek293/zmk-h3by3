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

## Debugging report

Two build errors were found and fixed:

### Bug 1 — Missing `matrix_transform.h` include in `h3by3.overlay`

**Error:**
```
devicetree error: h3by3.overlay:27 (column 13): parse error: expected number or parenthesized expression
```

**Root cause:** The `RC(row, col)` macro used inside the `map = < ... >` block is defined in `<dt-bindings/zmk/matrix_transform.h>`, but that header was not included in `h3by3.overlay`. The devicetree compiler therefore did not recognize `RC(0,0)` as a valid expression.

**Fix:** Added `#include <dt-bindings/zmk/matrix_transform.h>` at the top of `h3by3.overlay`.

---

### Bug 2 — No physical layout defined while `CONFIG_ZMK_STUDIO=y`

**Error:**
```
error: static assertion failed: "ISSUE FOUND: Keyboards require additional configuration to allow for
firmware with ZMK Studio enabled."
```

**Root cause:** `CONFIG_ZMK_STUDIO=y` was set in `h3by3.conf`, but the shield DTS did not define a `zmk,physical-layout` node. ZMK Studio requires at least one physical layout that describes the real-world position of every key; without it the build fails at compile-time with a static assertion.

**Fix:** Added `#include <physical_layouts.dtsi>` and a `zmk,physical-layout` node (`physical_layout_0`) to `h3by3.overlay`. The node lists all nine 1 u keys in a 3 × 3 grid with their x/y positions (in hundredths of a key unit), referencing the existing `default_transform` and `kscan0`.

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