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
2. **Download** the artifact: `gh run download` (or Actions tab → latest run). Produces `firmware/corne_left-nice_nano__zmk-zmk.uf2`, `firmware/corne_right-nice_nano__zmk-zmk.uf2`, and `firmware/settings_reset-nice_nano__zmk-zmk.uf2`.
3. **Flash a half**:
   - Plug that half into USB
   - Double-tap the reset button on the controller — it mounts as `/run/media/chris/NICENANO`
   - Copy the matching `.uf2` to `/run/media/chris/NICENANO/`
   - The drive auto-unmounts and the keyboard reboots into the new firmware
4. Repeat for the other half (when changing the keymap, both halves need the new firmware).

## Recovering from breakage

If the halves stop pairing or the keyboard goes weird, flash `settings_reset-nice_nano__zmk-zmk.uf2` onto each half (also produced by the build). That wipes saved Bluetooth pairings; re-pair after.

## Editing the layout

Edit `config/corne.keymap`. Keymap reference: <https://zmk.dev/docs/keymaps>. Common changes:

- Swap a homerow mod assignment: change `&hm LGUI A` to `&hm LALT A`, etc.
- Add a layer: copy a `*_layer` block, give it a new name, increment the layer index, and add a `&mo N` or `&lt N KEY` binding to trigger it.
- Add a combo: drop a new block under `combos`, give it a unique name and a key-position pair.

## Live editing without flashing — ZMK Studio

[ZMK Studio](https://zmk.studio) is **enabled** on this config (left half only — see `build.yaml`). It lets you remap keys, move layers around, and tweak combos live over USB, no rebuild/reflash needed. The changes persist on the keyboard and override the compiled keymap.

To use it:

1. Plug the **left** half into USB (Studio runs on the central half).
2. Open <https://zmk.studio> in Chrome/Edge (it uses WebSerial — Firefox won't work).
3. Click connect and pick the keyboard's serial device.
4. **Unlock for editing:** hold the NUM thumb key and tap the top-left key (TAB position) — that's `&studio_unlock`. It re-locks on idle.

**Linux note:** WebSerial needs read/write on the `/dev/ttyACM*` device. If the browser can't open it, add yourself to the `dialout` group (`sudo usermod -aG dialout $USER`, then re-login) — or `udevadm` a rule. This is the one Linux-specific gotcha for Studio.

The compiled `config/corne.keymap` remains the source of truth / baseline; commit substantive changes back here so a settings reset doesn't lose them.
