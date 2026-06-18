# zmk-temper

ZMK firmware for the Temper split keyboard, ported from
[`corne-zmk`](https://github.com/) (the Zuul layout). Hardware deltas vs. Corne:

- **5 columns per side** (Corne: 6). Outer column dropped on both sides.
- **No OLED**. `CONFIG_ZMK_DISPLAY` disabled.

Built on `nice_nano_v2`. ZMK Studio enabled — physical layout authored in
`config/temper.keymap` from PCB drill data (choc v1, 18×17mm pitch, column
stagger).

## Layers

| # | Name       | Activate                           |
|---|-----------|------------------------------------|
| 0 | Linux      | base                               |
| 1 | Arrows     | hold left thumb 2 (`SPACE`)        |
| 2 | Mouse & BT | hold left thumb 3 (`TAB`)          |
| 3 | Sound      | hold left thumb 1 (`ESC`)          |
| 4 | Numbers    | hold right thumb 1 (`RETURN`)      |
| 5 | Symbols    | hold right thumb 2 (`BACKSPACE`)   |
| 6 | F          | hold right thumb 3 (`DELETE`)      |
| 7 | Gaming     | tap `&to 7` on Mouse layer (row 0) |

### Linux (base)

```
╭─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────╮
│  Q  │  W  │  E  │  R  │  T  │   │  Y  │  U  │  I  │  O  │  P  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│  A* │  S* │  D* │  F* │  G  │   │  H  │  J* │  K* │  L* │  ;* │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│  Z  │  X  │  C  │  V  │  B  │   │  N  │  M  │  ,  │  .  │  /  │
╰─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┴─────╯
            │ ESC │ SPC │ TAB │   │ RET │ BSP │ DEL │
            ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

Hold-tap on home row (`rpi`, tap-preferred, 200ms term, 125ms prior-idle):

```
A = TAP A / HOLD LCTRL      J = TAP J / HOLD LSHIFT
S = TAP S / HOLD LALT       K = TAP K / HOLD LGUI
D = TAP D / HOLD LGUI       L = TAP L / HOLD LALT
F = TAP F / HOLD LSHIFT     ; = TAP ; / HOLD LCTRL
```

Thumb layer-taps (`&lt`):

```
ESC = TAP ESC / HOLD Sound      RET = TAP RET / HOLD Numbers
SPC = TAP SPC / HOLD Arrows     BSP = TAP BSP / HOLD Symbols
TAB = TAP TAB / HOLD Mouse&BT   DEL = TAP DEL / HOLD F
```

Dropped from Corne: `CAPS`, `LS(6)`, `LS(\`)`. `'` (SQT) relocated to Numbers
layer (row 1, col 9).

### Arrows

```
╭─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────╮
│  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │   │ ←   │  ↓  │  ↑  │  →  │  ▽  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │   │HOME │PGDN │PGUP │ END │  ▽  │
╰─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┴─────╯
            │  ▽  │  ▽  │  ▽  │   │ RET │ BSP │ DEL │
            ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

### Mouse & BT

```
╭─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────╮
│  ▽  │  ▽  │  ▽  │  ▽  │GAME │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│ BT0 │ BT1 │ BT2 │ BT3 │ BT4 │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│BCLR │  ▽  │  ▽  │  ▽  │  ▽  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
╰─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┴─────╯
            │  ▽  │  ▽  │  ▽  │   │  ▽  │  ▽  │  ▽  │
            ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

`GAME` = `&to 7`. `BCLR` = `&bt BT_CLR`. ZMK Studio is enabled on the left half only (via `cmake-args` in `build.yaml`). The top-left key (row 0, col 0) is `&studio_unlock` on the left build — hold left `TAB` then press it to unlock ZMK Studio for live keymap edits. On the right build it compiles as `&trans` (guarded by `#ifdef CONFIG_ZMK_STUDIO`).

### Sound

```
╭─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────╮
│  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │   │  ▽  │  ▽  │  ▽  │  "  │  ~  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │   │  ▽  │VOL+ │NEXT │  ▽  │  '  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │   │  ▽  │VOL- │PREV │  ▽  │  `  │
╰─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┴─────╯
            │  ▽  │  ▽  │  ▽  │   │STOP │PLAY │MUTE │
            ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

### Numbers

```
╭─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────╮
│  [  │  7  │  8  │  9  │  ]  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│  ;  │  4  │  5  │  6  │  =  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│  `  │  1  │  2  │  3  │  \  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
╰─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┴─────╯
            │  .  │  0  │  -  │   │  ▽  │  ▽  │  ▽  │
            ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

### Symbols

```
╭─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────╮
│  {  │  &  │  *  │  .  │  }  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│  :  │  $  │  %  │  ^  │  +  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│  ~  │  !  │  @  │  #  │  |  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
╰─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┴─────╯
            │  (  │  )  │  _  │   │  ▽  │  ▽  │  ▽  │
            ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

### F

```
╭─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────╮
│ F12 │ F7  │ F8  │ F9  │PSCR │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│ F11 │ F4  │ F5  │ F6  │  ▽  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│ F10 │ F1  │ F2  │ F3  │  ▽  │   │  ▽  │  ▽  │  ▽  │  ▽  │  ▽  │
╰─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┴─────╯
            │ ESC │ SPC │ TAB │   │  ▽  │  ▽  │  ▽  │
            ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

### Gaming

```
╭─────┬─────┬─────┬─────┬─────╮   ╭─────┬─────┬─────┬─────┬─────╮
│  Q  │  W  │  E  │  R  │  T  │   │  Y  │  U  │  I  │  O  │  P  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│  A  │  S  │  D  │  F  │  G  │   │  H  │  J  │  K  │  L  │  ;  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│  Z  │  X  │  C  │  V  │  B  │   │  N  │  M  │  ,  │  .  │  /  │
╰─────┴─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┴─────╯
            │ ESC │ SPC │ TAB │   │ RET │ BSP │ DEL │
            ╰─────┴─────┴─────╯   ╰─────┴─────┴─────╯
```

⚠ **Gaming layer is sticky** after this port: Corne's `&to (-1)` exit (col 11
row 0) and the `LALT`/`LSHIFT`/`LCTRL` mods (col 0 rows 0–2) were dropped with
the outer column. Add `&to 0` somewhere or unplug to exit.

## Macros

| Name         | Action                                        |
|--------------|-----------------------------------------------|
| `next_wp`    | Hold LGUI, tap LEFT — next workspace          |
| `prev_wp`    | Hold LGUI, tap RIGHT — prev workspace         |
| `ch_next_wp` | Hold LGUI+LSHIFT, tap LEFT — move ws + switch |
| `ch_prev_wp` | Hold LGUI+LSHIFT, tap RIGHT — move ws + switch |

## Custom behaviors

| Name  | Type      | Use                                                |
|-------|-----------|----------------------------------------------------|
| `rpi` | hold-tap  | Home-row mods; require-prior-idle 125ms            |
| `mpi` | hold-tap  | Mac-style hold-tap with `&trans` tap fallback      |

## Build & flash

CI builds both halves on push (see `.github/workflows/build.yml`):

1. Push to GitHub → Actions runs the matrix in `build.yaml`.
2. Download `firmware.zip` artifact — contains
   `temper_left-nice_nano_v2-zmk.uf2` and `temper_right-nice_nano_v2-zmk.uf2`.
3. Double-tap reset on each half → mounts as USB drive → drag the matching
   `.uf2` over.
4. Pair via Bluetooth: hold left `TAB` (Mouse&BT layer) + tap `&bt BT_SEL 0..4`.
