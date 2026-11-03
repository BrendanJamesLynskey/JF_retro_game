# 3. Power and battery

## The cell

- **One 3.7 V lithium-polymer (LiPo) cell, about 1200 mAh**, in a flat pouch around
  50 × 34 × 6 mm.
- It **must have a built-in protection circuit** (a small PCB under the tape at the
  wire end) against over-discharge, over-charge and short circuit.
- Connector: **JST-PH 2.0 mm, 2-pin**. *Check the polarity before plugging in.*
  Different sellers wire red and black the opposite way round, and a reversed cell
  can destroy the charger. Match the + mark on the board to the red wire, and swap
  the crimps over if they don't match.

## Charging and on/off

### Build 1: Pimoroni LiPo SHIM for Pico

A thin board that solders under the Pico. It:

- charges the cell from the Pico's own USB port,
- feeds the cell to VSYS,
- adds a **power button** (press to turn on, hold to turn off), and
- has charge and power LEDs.

This is the quickest safe route, and it's the one the first build uses.

### Build 2: discrete parts (optional, for a custom board)

```
USB-C 5 V ──┬── VBUS (Pico)
            └── MCP73831 charger ── LiPo + ──┬── P-MOSFET "ideal diode" ── VSYS
                (R_prog 2.2k ≈ 450 mA)       └── (cell also feeds the charger's sense)
Slide switch: 3V3_EN to GND = off
```

- The Pico datasheet's section on powering the board shows this exact charger and
  ideal-diode circuit. Follow it rather than inventing one.
- **Off switch:** pulling the Pico's **3V3_EN** pin to GND turns off its 3.3 V
  regulator, so the whole console (screen, amp, Pico) goes dead. A tiny slide
  switch between 3V3_EN and GND makes a true off switch without switching the
  battery current. A small current still flows while it's off; measure it, and if
  it's too high, switch the battery line instead.
- Charge at no more than 1 C (1200 mA for this cell). 450 mA is gentle and keeps
  the charger cool inside a closed case.

## Battery gauge

On a plain Pico 2, **GPIO29 / ADC3 reads VSYS through a divide-by-3**:

```python
from machine import ADC
vsys = ADC(29).read_u16() * 3 * 3.3 / 65535
```

| VSYS (no load, roughly) | Show |
|---|---|
| > 4.0 V | 4 bars |
| 3.8–4.0 V | 3 bars |
| 3.6–3.8 V | 2 bars |
| 3.45–3.6 V | 1 bar, flashing |
| < 3.4 V | "Battery low", save high scores and sleep |

When USB is plugged in, VSYS reads about 5 V (minus a diode drop), so treat
> 4.4 V as "charging". Average readings over a second, because they sag while the
speaker plays.

## Power budget (estimates, to be measured)

| Part | Current from the cell |
|---|---|
| Pico 2 running a game at 150 MHz | ~25 mA |
| 2.0" IPS screen, backlight at 70 % | ~35 mA |
| Amp + speaker, average during play | ~15 mA |
| Rumble motor, average (short buzzes) | ~5 mA |
| Regulator and LEDs | ~5 mA |
| **Total** | **~85 mA** |

A 1200 mAh cell with about 85 % usable gives 1020 mAh ÷ 85 mA ≈ **12 hours**.
Call it ten to be safe. The screen backlight is the biggest thing we control: a
brightness setting in the menu (PWM on the backlight pin) is the best way to
stretch battery life.

**Measure it.** Put a USB power meter inline while charging, and run a "battery
burn" test: the console loops a demo game until the low-battery screen shows, and
logs the minutes to flash.

For comparison, a Pi Zero 2 W console draws about 2 W. From the same cell that is
4.4 Wh × 0.85 (boost converter) ÷ 2 W ≈ 2 hours.

## Safety

- Protected cells only. Never use a cell that is puffy, dented or punctured.
- The cell sits in its own pocket in the case, away from screws and sharp edges,
  held with foam tape, not glue.
- Charge only through the charger board, never straight from a bench supply.
- For the first few charges, charge while someone is in the room, on a hard surface.
- Keep the battery leads short and the joints covered in heat-shrink.
- An adult checks the polarity and the first power-up.
