# zmk-config — Corne 42 LP

ZMK firmware config for chris's Corne 42 LP (nice!nano v2 controller, OLED + RGB).

## Layout

Mirrors the Kanata layout at [`~/.config/kanata/kanata.kbd`](https://github.com/cobalt-craft/.config/blob/develop/kanata/kanata.kbd) as closely as a 42-key board allows.

- **Homerow mods** on ASDF/JKL; (GUI/ALT/CTRL/SHIFT, mirrored across hands)
- **ESC/NAV** on the key to the left of A (tap = ESC, hold = NAV layer with arrows on HJKL) — direct mirror of Caps Lock behavior in Kanata
- **`\`/NUM** on the right-inner thumb (tap = `\`, hold = NUMROW layer) — direct mirror of `\` behavior in Kanata
- **LSFT + RSFT → BSPC** combo

See [`config/corne.keymap`](config/corne.keymap) for the full layer maps with ASCII diagrams.

## Build & flash

1. **Push** any keymap change to this repo. GitHub Actions builds two `.uf2` files automatically (~5 min).
2. **Download** the artifact: Actions tab → latest run → `firmware.zip`. Unzip to get `corne_left-nice_nano_v2-zmk.uf2` and `corne_right-nice_nano_v2-zmk.uf2`.
3. **Flash a half**:
   - Plug that half into USB
   - Double-tap the reset button on the controller — it mounts as `/run/media/chris/NICENANO`
   - Copy the matching `.uf2` to `/run/media/chris/NICENANO/`
   - The drive auto-unmounts and the keyboard reboots into the new firmware
4. Repeat for the other half (when changing the keymap, both halves need the new firmware).

## Recovering from breakage

If the halves stop pairing or the keyboard goes weird, flash `settings_reset-nice_nano_v2-zmk.uf2` onto each half (also produced by the build). That wipes saved Bluetooth pairings; re-pair after.

## Editing the layout

Edit `config/corne.keymap`. Keymap reference: <https://zmk.dev/docs/keymaps>. Common changes:

- Swap a homerow mod assignment: change `&hm LGUI A` to `&hm LALT A`, etc.
- Add a layer: copy a `*_layer` block, give it a new name, increment the layer index, and add a `&mo N` or `&lt N KEY` binding to trigger it.
- Add a combo: drop a new block under `combos`, give it a unique name and a key-position pair.

## Live editing without flashing (optional)

[ZMK Studio](https://zmk.studio) supports runtime keymap edits over USB for ZMK builds with the `studio-rpc-usb-uart` snippet. Not enabled in this config yet — flip on by adding `snippet: studio-rpc-usb-uart` to each entry in `build.yaml` and adding `CONFIG_ZMK_STUDIO=y` to `corne.conf`.
