# 2. Choosing the brain

Four candidates, all Raspberry Pi hardware.

| | Pico (RP2040) | **Pico 2 (RP2350)** | Pi Zero 2 W | Pi 5 |
|---|---|---|---|---|
| CPU | 2 × Cortex-M0+ at 133 MHz | 2 × Cortex-M33 at 150 MHz, with FPU | 4 × Cortex-A53 at 1 GHz | 4 × Cortex-A76 at 2.4 GHz |
| RAM | 264 KB | 520 KB | 512 MB | 2–16 GB |
| Storage | 2 MB flash | 4 MB flash | microSD | microSD / NVMe |
| Time to playing | < 1 s | < 1 s | ~15–30 s (Linux boot, can be trimmed) | ~15–30 s |
| Typical power, whole console | ~0.3–0.5 W | ~0.3–0.5 W | ~1.5–2.5 W | 5–10 W |
| Battery wiring | LiPo straight to VSYS | LiPo straight to VSYS | needs a 5 V boost converter | needs a large 5 V/5 A pack |
| Run time on one 1200 mAh cell (rough) | ~10 h | ~10 h | ~2 h | not practical |
| Must shut down safely | no | no | yes (SD card corruption) | yes |
| Software | MicroPython, C/C++ | MicroPython, C/C++ | Python/Pygame, PICO-8, anything Linux | anything Linux |
| Board price (rough) | £4 | £5 | £15 | £50+ |

Run times are estimates, worked out in [Power and battery](03_power_and_battery.md).

## Decision: Pico 2 (RP2350)

The Pico 2 wins on the things that make a handheld fun to own:

- **Instant on, instant off.** No boot, no shutdown and no corrupted SD card when the
  battery runs flat in the middle of a game.
- **Long run time from a small cell**, so the case can be small.
- **Simple power.** The Pico's VSYS input takes 1.8–5.5 V, so a LiPo cell can feed it
  directly with no boost converter.
- **Enough grunt for 160×120 at 60 fps.** The FPU and the extra RAM over the RP2040
  matter here. 520 KB holds two full-size 320×240 16-bit frame buffers (2 × 150 KB),
  so later games can draw at full resolution if they need to.
- **PIO and DMA** stream pixels to the screen in the background while the CPU works
  out the next frame.
- **It forces small, finishable games**, which is rule 8 in
  [What makes it fun](01_what_makes_it_fun.md).

Use the plain **Pico 2**, not the Pico 2 W. Wireless isn't needed, and on the W
models GPIO29 (used to read battery voltage) is shared with the radio.

## When to pick something else

- **RP2040 Pico**: fine if one is already in the drawer. Draw at 160×120 only (one
  320×240 buffer would use 150 KB of the 264 KB), and expect MicroPython to be
  noticeably slower.
- **Pi Zero 2 W**: pick this for a "fantasy console" handheld running PICO-8,
  TIC-80 or Pygame, or for games that need a lot of memory. You pay for it with a boost
  converter, a safe-shutdown circuit, a bigger battery and a 20-second boot. It's
  worth keeping as a possible second build later. It drives the same SPI screen.
- **Pi 5**: too power-hungry for a handheld. It makes an excellent *development*
  machine, though, and a Pi 5 on the TV is already the family's game box.

## Ready-made alternatives (for comparison)

Pimoroni's **PicoSystem** is a finished RP2040 handheld, and the **Thumby** is a
keyring-sized one. Both are worth looking at for ideas about button feel and case
shape. Building our own gets us a bigger screen, rumble, a link port and the fun of
making it.
