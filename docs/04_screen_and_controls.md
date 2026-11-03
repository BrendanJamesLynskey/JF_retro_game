# 4. Screen and controls

## Screen

**2.0-inch 320×240 IPS LCD, ST7789 controller, SPI.**

- IPS means the colours don't wash out when the console is tilted.
- 320×240 gives a 4:3 retro shape and doubles cleanly from 160×120.
- It is the panel on the Pimoroni **Pico Display Pack 2.0** (used in build 1) and is
  sold on its own as a bare module for later builds.
- A 2.4" ILI9341 panel at the same resolution also works if a bigger screen is wanted.
  The same code works with a different setup sequence.

### Frame-rate sums

The screen is fed over SPI. ST7789 panels usually run reliably at about 62.5 MHz,
which is faster than the datasheet figure; that has to be tested on our panel.

| What is sent | Bits per frame | Time at 62.5 MHz | Max fps |
|---|---|---|---|
| Full 320×240, 16-bit colour | 1,228,800 | 19.7 ms | ~50 |
| 160×120 drawn, doubled while sending | 1,228,800 on the wire | 19.7 ms | ~50 |
| Only the changed rows ("dirty rows") | less | less | 60 |

So the plan is:

- **Draw at 160×120 into a 38.4 KB buffer** (RGB565). It's quick to draw, tiny in
  RAM, and chunky enough to look retro.
- **Double each pixel on the way out.** The PIO can repeat each pixel and each row
  as it streams the buffer, and DMA feeds it without the CPU. While one frame is
  going out, the CPU draws the next one into a second buffer.
- **Target 50 fps with every frame sent in full, or 60 fps using dirty rows.**
  Either feels smooth. A steady 30 fps would feel worse than a steady 50.
- The ST7789 has a tearing-effect (TE) output pin. If the module exposes it, wiring
  it to a GPIO lets frames start at the right moment and removes tearing. This is a
  nice-to-have.

### Palette

Games use a fixed **16-colour palette** chosen once for the whole console, so every
game looks like it belongs to the same machine. Storing pixels as 4-bit palette
indices and converting them to RGB565 on output would halve the buffer again. That
can wait for a later version.

## Controls

```
   ┌──────────────────────────────────────────┐
   │  ┌────────────────────────────────────┐  │
   │  │                                    │  │
   │  │          2.0" screen 4:3           │  │
   │  │                                    │  │
   │  └────────────────────────────────────┘  │
   │     ▲                                    │
   │   ◀   ▶                        (B) (A)   │
   │     ▼                                    │
   │           [Select]  [Start]      ●spk    │
   └──────────────────────────────────────────┘
        power button + USB-C on the top edge
```

Landscape layout, with the screen on top and the controls below. This is a narrow
case (about 95 × 75 mm) that fits a pocket. A wider layout with the controls either
side of the screen is a later option, if it feels better to hold.

| Control | Part | Notes |
|---|---|---|
| D-pad | 4 × 6×6 mm tactile switches under a 3D-printed cross on a pivot nub | The nub stops two opposite directions being pressed at once |
| A, B | 12×12 mm tactile switches with round caps | Set at a slight angle, B lower-left of A, so the thumb rolls between them |
| Start, Select | 6×6 mm tactile switches, small pill caps | Recessed so they aren't pressed by accident |
| Power | the LiPo SHIM button (build 1), or a slide switch on 3V3_EN (build 2) | On the top edge |

Better feel later: harvest the **silicone membrane and conductive pads from a cheap
USB gamepad**, and print a PCB-style pad layout on perfboard with exposed copper.
It's softer and quieter, and feels like a real console. Try both and keep the one
that feels better.

### Reading the buttons

- Each switch goes from a GPIO to GND, using the internal pull-up, so pressed = 0.
- Read all of them once per frame, at the top of the game loop.
- Debounce in software: a press counts after two frames in a row agree. At 50 fps
  that's 40 ms, which isn't noticeable.
- Give each game "just pressed" and "just released" as well as "held", so menus and
  jumps don't repeat on their own.
- Holding **Start + Select for 2 seconds** returns to the game menu, from any game.
