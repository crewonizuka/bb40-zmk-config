# BB40 + PillBug ZMK config (with ZMK Studio)

A `zmk-config` repo for the MechWild BB40, built for the PillBug (BLE,
nRF52840), with a real BlackPill (F411CE) build included as a fallback.
ZMK Studio is enabled on both.

No fork of ZMK is needed — the PillBug board is already merged into
mainline ZMK (`west.yml` just points at `zmkfirmware/zmk`).

## Before your first build

Fill in **`PIN_MAPPING_WORKSHEET.md`** — it's one lookup against a ZMK docs
diagram, explained there. Then edit the `#define`s at the top of
`boards/shields/bb40/bb40.overlay` to match.

## Building

1. Create a new GitHub repo and push this folder's contents to it.
2. GitHub Actions will build automatically (see `.github/workflows/build.yml`
   and `build.yaml`). Check the **Actions** tab for the run.
3. Download `firmware.zip` from the completed run. It contains:
   - `bb40-pillbug-studio.uf2` — everyday firmware, PillBug, Studio enabled
   - `bb40-pillbug-settings-reset.uf2` — flash once if BLE pairing gets stuck
   - `bb40-blackpill-f411ce-studio.uf2` — wired build, if you swap boards later

## Flashing

Put the PillBug into bootloader mode (double-tap reset), it'll show up as a
USB drive, then drag the `.uf2` file onto it.

## Using ZMK Studio

1. Add `&studio_unlock` to your keymap on a key you can reach (already done,
   on the Fn layer in this starter keymap) and flash it once.
2. Connect the keyboard over USB, go to <https://zmk.studio/> (or the
   desktop app), and unlock it by pressing that key combo.
3. From there you can remap keys live, no reflashing needed — this is the
   whole point of the exercise.

## Customizing the keymap

The starter `bb40.keymap` is a reasonable-effort QWERTY layout, but the
bottom row on this board has a couple of unusually wide keys, so treat
those legends as a guess. Once flashed, finish it either:

- **In ZMK Studio** directly (recommended now that it can talk to the
  board), or
- **In the web-based keymap editor** built earlier in this conversation —
  export a `.keymap` block from there and paste it into
  `boards/shields/bb40/bb40.keymap`, replacing the `keymap { ... }` node.
  The binding order there needs to match this board's physical order
  (44 keys, row by row) — the editor's "Custom flat list" board preset
  with 44 keys will do that.
