# 6. Pin map and wiring

## Build 1: Pico 2 + Pico Display Pack 2.0 + LiPo SHIM

The only soldering is the Pico's header pins, which go through the LiPo SHIM
underneath. The Display Pack plugs on top.

The Display Pack's pins are fixed by the board (from Pimoroni's documentation, so
check them against the current pinout before relying on them):

| Display Pack function | GPIO |
|---|---|
| LCD DC | GP16 |
| LCD CS | GP17 |
| LCD SCLK (SPI0) | GP18 |
| LCD MOSI (SPI0) | GP19 |
| LCD backlight | GP20 |
| Button A | GP12 |
| Button B | GP13 |
| Button X | GP14 |
| Button Y | GP15 |
| RGB LED R/G/B | GP6 / GP7 / GP8 |

Build 1 has only four buttons, so the first games are designed for **four buttons**:
for example X/Y = left/right, A = action, B = jump, and A+B together = pause. That's
a fine limit for a first game. The D-pad arrives in build 2.

## Build 2: hand-wired console

The screen keeps the Display Pack's pins, so the same code runs on both builds.

| Function | GPIO | Notes |
|---|---|---|
| D-pad Up | GP2 | to GND, internal pull-up |
| D-pad Down | GP3 | 〃 |
| D-pad Left | GP4 | 〃 |
| D-pad Right | GP5 | 〃 |
| A | GP9 | 〃 |
| B | GP10 | 〃 |
| Start | GP11 | 〃 |
| Select | GP12 | 〃 |
| Link UART TX (UART0) | GP0 | 1 kΩ series resistor to the TRRS tip |
| Link UART RX (UART0) | GP1 | 1 kΩ series resistor from the TRRS ring 1 |
| Link "cable present" | GP13 | TRRS ring 2, pull-up; the cable shorts it to GND |
| Rumble | GP14 | to the transistor base through 1 kΩ |
| Amp shutdown (SD) | GP15 | high = on |
| LCD DC | GP16 | |
| LCD CS | GP17 | |
| LCD SCLK (SPI0) | GP18 | |
| LCD MOSI (SPI0) | GP19 | |
| LCD backlight (PWM) | GP20 | brightness setting |
| LCD TE (optional) | GP21 | tear-free frame start |
| Audio PWM out | GP22 | through RC filter to amp IN+ |
| LCD reset (optional) | GP26 | or tie to RUN |
| Accelerometer SDA / SCL (later) | GP6 / GP7 | I²C1 |
| Status LED (later) | GP8 | |
| Light sensor (later) | GP27 / ADC1 | |
| Battery voltage | GP29 / ADC3 | internal VSYS ÷ 3, plain Pico 2 only |
| Spare | GP28 | |

Power:

| Net | Connects |
|---|---|
| VSYS | LiPo SHIM or charger output → amp VIN, Pico VSYS |
| 3V3(OUT) | LCD VCC, accelerometer, pull-ups |
| GND | everything (use a star point near the Pico's GND pins for the amp) |
| 3V3_EN | slide switch to GND = off (build 2b, discrete power only) |

### Audio filter

```
GP22 ──[1 kΩ]──┬── amp IN+
               │
             [10 nF]
               │
GND ───────────┴── amp IN−
```

### Rumble driver

```
VSYS ──────┬───────────┐
           │           │
        (motor)   1N4148, cathode (stripe) to VSYS
           │           │
           ├───────────┘
           │
           C
GP14 ─[1 kΩ]─ B   2N2222
           E
           │
GND ───────┘
```

## Wiring rules

- Use **solid-core wire on the perfboard** and **stranded silicone wire** for
  anything that moves (battery, speaker, motor, TRRS socket).
- Keep the SPI wires to the screen short, under about 10 cm. Long SPI wires are the
  usual cause of a garbled picture at 62.5 MHz. If the picture glitches, drop the
  clock to 40 MHz first.
- Run the amp's and the motor's ground returns straight to a Pico GND pin, not
  through the screen's ground, to stop buzzing in the speaker.
- Tie down every wire that crosses the hinge line between the case halves.
