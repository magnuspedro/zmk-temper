# AGENTS.md — Temper ZMK config

ZMK firmware for the **Temper** split keyboard (nice_nano_v2), ported from the
Corne Zuul layout. The only files an agent normally needs to touch are:

| File | Purpose |
|------|---------|
| `config/temper.keymap` | All layers, macros, and behaviors |
| `config/temper.conf` | Kconfig flags (sleep, pointing, Studio, BLE) |
| `build.yaml` | CI build matrix (board/shield pairs) |
| `config/west.yml` | ZMK version pin (`v0.3`) and upstream temper shield module |
| `README.md` | Layer diagrams — keep in sync with keymap changes |

## Hardware facts

- **5 columns per side** (Corne has 6 — outer column dropped).
- **36 keys total**: 3 rows × 10 cols + 6 thumb keys.
- **No OLED** (`CONFIG_ZMK_DISPLAY` is disabled).
- **ZMK Studio** enabled on the left half via `studio-rpc-usb-uart` snippet.
- Keyboard name: `ZuulBoard`.

## Keymap layout (index reference)

Positions are **0-indexed**, left-to-right, top-to-bottom:

```
Row 0:  cols 0–9   (top row)
Row 1:  cols 0–9   (home row)
Row 2:  cols 0–9   (bottom row)
Thumb:  6 keys — left thumb cols 0–2, right thumb cols 3–5
```

Split visualisation (col 0 = far left, col 9 = far right):

```
╭─col0──col1──col2──col3──col4─╮   ╭─col5──col6──col7──col8──col9─╮
│  row 0                        │   │                               │
│  row 1  (home)                │   │                               │
│  row 2                        │   │                               │
╰───────┬──────┬──────╮         │   │         ╭──────┬──────┬───────╯
        │thumb0│thumb1│thumb2   │   │  thumb3 │thumb4│thumb5│
        ╰──────┴──────┴─────────╯   ╰─────────┴──────┴──────╯
```

Thumb layer-taps (base layer):

```
thumb0 = ESC  / hold → layer 3 (Sound)
thumb1 = SPC  / hold → layer 1 (Arrows)
thumb2 = TAB  / hold → layer 2 (Mouse & BT)
thumb3 = RET  / hold → layer 5 (Symbol)
thumb4 = BSPC / hold → layer 4 (Number)
thumb5 = DEL  / hold → layer 6 (F-keys)
```

## Layer summary

| Layer | Name       | Activate              |
|-------|------------|-----------------------|
| 0     | Linux      | base                  |
| 1     | Arrows     | hold SPC              |
| 2     | Mouse & BT | hold TAB              |
| 3     | Sound      | hold ESC              |
| 4     | Number     | hold BSPC             |
| 5     | Symbol     | hold RET              |
| 6     | F-keys     | hold DEL              |
| 7     | Gaming     | `&to 7` on Mouse & BT |

### Number layer right-side additions (cols 5–9)

```
row 0, col 8 → "  (DQT)
row 0, col 9 → ~  (TILDE)
row 1, col 8 → '  (SQT)
row 1, col 9 → ^  (CARET)
row 2, col 9 → `  (GRAVE)
```

Everything else on the right half of Number/Symbol/F-key/Arrow layers is
`&trans` unless noted in the keymap.

## Common ZMK keycode patterns used here

- `&kp KEY` — standard key press
- `&lt LAYER KEY` — layer-tap (hold = layer, tap = key)
- `&rpi MOD KEY` — hold-tap with require-prior-idle (home-row mods)
- `&bt BT_SEL N` — select Bluetooth profile N
- `&bt BT_CLR` — clear current Bluetooth bond (Mouse & BT layer, row 2 col 0)
- `&mmv MOVE_*` — mouse movement
- `&mkp MB*` — mouse button
- `&to N` — switch to layer N permanently
- `&studio_unlock` — unlock ZMK Studio editing

## Making keymap changes

1. Edit `config/temper.keymap`.
2. Match the ASCII diagram style in `README.md` — update the relevant layer
   diagram and any prose notes in the same commit.
3. Column indices start at 0. Rows start at 0 (top). Thumb keys follow row 2
   in source order (left-thumb keys first, then right-thumb keys).
4. Both halves share one keymap file; the shield module handles split routing.

## Build & CI

Firmware is built by GitHub Actions on every push:

- Left half: `temper_left` + `studio-rpc-usb-uart` snippet (ZMK Studio).
- Right half: `temper_right`.
- Also builds `settings_reset` for factory-reset UF2.

Workflow delegates entirely to `zmkfirmware/zmk/.github/workflows/build-user-config.yml@v0.3`. No local build toolchain is needed — push and download the artifact.

## Flashing

1. Double-tap reset → half mounts as USB drive.
2. Drag the matching `.uf2` from `firmware.zip` artifact.
3. Pair: hold TAB (Mouse & BT layer) + tap `BT_SEL 0–4`.
4. Clear a bond: hold TAB + press bottom-left key (`BT_CLR`, row 2 col 0).
