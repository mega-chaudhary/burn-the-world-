# ASCII Candle

A match is struck and lights a candle. The flame grows, then the candle tips
over and the floor catches fire. Everything is drawn with coloured ASCII
characters in a tkinter window titled "Candle".

## Run

```bash
pip install -r requirements.txt
python candle.py
```

Keys: **R** restarts, **Space** pauses, **Esc** quits.

Python 3.8+ is required. tkinter comes with the standard Windows and macOS
installers. On Linux you may need `sudo apt install python3-tk`.

## Export to GIF (optional)

```bash
pip install pillow
python candle.py --export candle.gif --seconds 14 --fps 20
```

## Tweaking

All the knobs are at the top of `candle.py`:

| Setting | What it does |
|---|---|
| `COLS`, `ROWS` | grid size (more cells = finer image, lower fps) |
| `T_STRIKE`, `T_LIGHT`, `T_OUT` | when the match strikes, lights the wick, goes out |
| `T_TIP`, `T_FALL` | when the candle tips over and how long the fall takes |
| `CAND_W`, `CAND_H`, `CAND_X` | candle size and position |
| `RAMP` | character ramp from dim to bright |
| `FPS` | target frame rate |

## How it works

- Each frame, `Scene.frame(t, dt)` computes a brightness and a colour for
  every cell using numpy. It works in layers: light field, background,
  floor, candle body, the white-hot floor band, flames, then particles.
- Brightness picks a glyph from `RAMP`. Colour blends deep red → orange →
  white as the heat rises.
- The flames are tear-drop shapes distorted by scrolling fractal noise. The
  floor fire spreads outward from where the wick hits the ground.
- The tkinter front end creates one canvas text item per cell and only
  updates the cells that changed since the last frame. That keeps it smooth
  in pure Python.
