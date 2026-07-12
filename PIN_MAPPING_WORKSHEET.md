# BB40 pin mapping worksheet

The shield file (`boards/shields/bb40/bb40.overlay`) references pins through
the shared **blackpill** interconnect used by both real BlackPill boards and
the PillBug: `&blackpill N`. This lets one shield file build for either
board. `N` is **not** the STM32 pin number — it's an index into a fixed
pinout diagram that ZMK publishes as an image (not text), so it has to be
read off visually rather than looked up automatically.

## What to do

1. Open <https://zmk.dev/docs/development/hardware-integration/new-shield#kscan>
2. Under "Kscan", click the **BlackPill Shields** tab to reveal the pinout
   diagram. It's the same diagram whether you're building for the PillBug
   or a real BlackPill — that's the point of the shared interconnect.
3. For each pin name below (taken directly from your `info.json`), find that
   label on the diagram and write down the number next to it.
4. Open `boards/shields/bb40/bb40.overlay` and replace each `0` placeholder
   in the `#define BB40_COL*` / `BB40_ROW*` / `BB40_ENC_*` block with the
   number you found.

| Purpose         | STM32 pin (from info.json) | `&blackpill` index | `#define` to edit |
|-----------------|-----------------------------|---------------------|--------------------|
| Column 0        | B10                          |                     | `BB40_COL0`        |
| Column 1        | B1                           |                     | `BB40_COL1`        |
| Column 2        | B0                           |                     | `BB40_COL2`        |
| Column 3        | A7                           |                     | `BB40_COL3`        |
| Column 4        | A6                           |                     | `BB40_COL4`        |
| Column 5        | A5                           |                     | `BB40_COL5`        |
| Column 6        | A4                           |                     | `BB40_COL6`        |
| Column 7        | A3                           |                     | `BB40_COL7`        |
| Column 8        | A2                           |                     | `BB40_COL8`        |
| Column 9        | A1                           |                     | `BB40_COL9`        |
| Column 10       | A0                           |                     | `BB40_COL10`       |
| Column 11       | C15                          |                     | `BB40_COL11`       |
| Row 0           | B12                          |                     | `BB40_ROW0`        |
| Row 1           | B13                          |                     | `BB40_ROW1`        |
| Row 2           | B14                          |                     | `BB40_ROW2`        |
| Row 3           | B15                          |                     | `BB40_ROW3`        |
| Encoder A       | B4                           |                     | `BB40_ENC_A`       |
| Encoder B       | B3                           |                     | `BB40_ENC_B`       |

That's 18 lookups, all from one diagram. Everything else in this repo
(matrix layout, key positions for ZMK Studio, keymap, encoder config, board
metadata) is already derived directly from your `info.json` and doesn't
need any guessing.

## Why this one thing needs manual lookup

Every other file in this repo could be generated straight from your
`info.json` and ZMK's published documentation. This mapping is the one
exception: ZMK only publishes it as an image in their docs, not as a
lookup table, config file, or anything else fetchable as text. Once you
fill in the 18 numbers above, the whole thing should build.

## Not yet included

- The BB40's caps/num/scroll-lock **indicator LEDs** (pins B8/B7/B9 in your
  info.json) aren't wired up yet. ZMK can drive HID lock indicators through
  GPIO, but the exact devicetree binding has changed across ZMK versions,
  so it's worth confirming the current syntax before adding it rather than
  guessing. Everything else (matrix, encoder, Studio) works without it.
