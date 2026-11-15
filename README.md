# ZMK config template (customkeyboards)

A [zmk-config][zmk-config] template that builds your firmware against
[danielsvane/zmk][fork] instead of upstream ZMK, and hands you the `.uf2` as a
GitHub Actions artifact. You don't need a local toolchain.

This fork adds editing of combos and custom behaviours at runtime, over the ZMK
Studio protocol -- things upstream ZMK Studio can't express yet.

## You also need the matching Studio client

The fork adds RPC subsystems that the official client at zmk.studio does not
know about. Firmware built from here will connect to the official client, but
the combo and behaviour editors won't be there. Use

**<https://studio.customkeyboards.eu>**

which is built against the same protocol.

## Usage

1. Click **Use this template** to create your own repo. Name it `zmk-config` by
   convention.
2. Add your keymap and shield under `config/`, or a whole board definition under
   `boards/` -- the repo root is a Zephyr module, so `boards/` is on the board
   root path.
3. List what to build in [`build.yaml`](build.yaml). It ships fully commented
   out, so the first build does nothing until you fill it in.
4. Commit and push. The **Actions** tab will have a run; open it and download
   `firmware.zip` from the **Artifacts** section at the bottom.
5. Flash the `.uf2`, then open <https://studio.customkeyboards.eu> and connect.

### Turning on Studio

Studio is opt-in per build. Add the snippet and the config flag:

```yaml
include:
  - board: nice_nano_v2
    shield: corne_left
    snippet: studio-rpc-usb-uart
    cmake-args: -DCONFIG_ZMK_STUDIO=y
  - board: nice_nano_v2
    shield: corne_right
```

On a split, only the central side gets them.

## Caveats

This template tracks the fork's `customkeyboards` branch, which moves. Two
builds from the same config a month apart may not be the same firmware, and a
protocol change on the branch can put your firmware and the Studio client out
of step. Re-flash from a fresh build if the client starts behaving oddly.

Everything else works the way upstream ZMK does -- see the [ZMK docs][docs] for
keymaps, shields, and behaviours. Report problems with the fork here rather
than to the ZMK project.

[zmk-config]: https://zmk.dev/docs/customization
[fork]: https://github.com/danielsvane/zmk/tree/customkeyboards
[docs]: https://zmk.dev/docs
