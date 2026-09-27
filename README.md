# borochid-logitech-g502x

Borochid device package for the **Logitech G502 X LIGHTSPEED** mouse (on its
LIGHTSPEED receiver).

This repo is **data only**: `logitech.g502x/manifest.json` and the mouse's
picture. It is published, signed, to the Borochid registry, and the service
downloads it when a matching mouse appears. The behaviour comes from the
[`logitech-hidpp` driver](https://github.com/RalphJS/borochid-driver-logitech-hidpp),
a system package the GUI offers to install through PackageKit the first
time it's needed.

**License:** Apache-2.0, except the mouse's picture, a drawing based on Logitech's
product photography, which is not covered by it. See [NOTICE](NOTICE).

## What's in the manifest

| Section | Purpose |
|---|---|
| `match` | `046d:409f`: the mouse as the receiver's kernel driver presents it. Borochid detects devices paired to a receiver as devices of their own. |
| `channel` | HID: the mouse's own hidraw node under the receiver. |
| `driver` | `logitech-hidpp`, compatible versions, and the system package that provides it. |
| `power_supply` | Battery from the kernel (`hid-logitech-hidpp` already reads it); no device access. |
| `input` | The service's virtual input device, for buttons bound to shortcuts or scrolling. |
| `hidpp` | The model profile: the 11 buttons in the order the mouse reports them (bit n-1 = button n), which can be remapped, what each does by default, and the starting DPI stages, shift DPI and report rate. |
| `display_name` | `Logitech G502 X LS` (as the receiver names it) becomes **Logitech G502 X LIGHTSPEED**. |
| `category`, `image` | `mouse`, and its picture. PNG, at most 384×384 and 256 KiB; `make check` enforces it. |
| `battery`, `available` | Battery level and charging from the kernel; the mouse is usable while the kernel can reach it. |
| `summary` | The status line: the active profile's name, or "Asleep". |
| `ui` | Profile settings (DPI stages, DPI shift, report rate) and the button list. Profiles themselves are switched at the top right of the Borochid window. |

### Buttons

| # | Id | Button | Default |
|---|---|---|---|
| 1 | `left` | Left click | (not remappable) |
| 2 | `right` | Right click | Right click |
| 3 | `middle` | Wheel click | Middle click |
| 4 | `g4` | G4 (back) | Back |
| 5 | `g6` | G6 (sniper) | DPI shift |
| 6 | `g5` | G5 (forward) | Forward |
| 7 | `tilt_left` | Wheel left | Scroll left |
| 8 | `tilt_right` | Wheel right | Scroll right |
| 9 | `g9` | G9 | Nothing |
| 10 | `g8` | G8 | DPI up |
| 11 | `g7` | G7 | DPI down |

The ratchet switch behind the wheel is mechanical and isn't reported.

## Working on it

```sh
python3 -m venv --system-site-packages .venv
.venv/bin/pip install -e ../borochid/packages/common -e ../borochid/packages/service -e ../borochid-driver-logitech-hidpp
make check
make install-local        # the service now uses this copy, no registry needed
```

## Publishing

```sh
make publish REGISTRY=../registry-out KEY=/path/to/publisher.key
```
