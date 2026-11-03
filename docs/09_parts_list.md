# 9. Parts list

Prices are **rough UK prices** from memory (Pimoroni, The Pi Hut, Amazon, AliExpress)
to give an idea of the total. Check current prices before ordering.

## Build 1: playable on the desk, then on battery

| Part | Qty | Rough price | Notes |
|---|---|---|---|
| Raspberry Pi Pico 2 + a pair of 20-pin male headers | 1 | £6 | Plain Pico 2, not the W |
| Pimoroni Pico Display Pack 2.0 | 1 | £19 | 320×240 IPS, 4 buttons, RGB LED |
| Pimoroni LiPo SHIM for Pico | 1 | £11 | Charger and power button |
| LiPo cell, ~1200 mAh, protected, JST-PH | 1 | £9 | **Check polarity** |
| USB cable for the Pico (micro-USB) | 1 | — | Probably already in a drawer |
| **Build 1 total** | | **~£45** | |

(The LiPo SHIM sits flat under the Pico and the header pins are soldered through
both boards, so buy the Pico 2 without headers fitted and follow Pimoroni's fitting
guide.)

## Build 2: the handheld

Adds to build 1 (it reuses the Pico 2, LiPo SHIM and cell; the Display Pack goes
back on the desk for development, and a bare screen goes in the case):

| Part | Qty | Rough price | Notes |
|---|---|---|---|
| 2.0" 320×240 ST7789 IPS SPI module | 1 | £8 | Pick one with the TE pin broken out if possible |
| 6×6 mm tactile switches (assorted heights) | 20 | £3 | D-pad, Start, Select, spares |
| 12×12 mm tactile switches with round caps | 4 | £3 | A, B, spares |
| PAM8302 mono amp breakout | 1 | £4 | |
| 28 mm 8 Ω 0.5 W speaker | 1 | £2 | |
| 10 mm coin vibration motor, 3 V | 2 | £2 | One spare |
| 2N2222 transistors, 1N4148 diodes, 1 kΩ and 10 kΩ resistors, 10 nF caps | some | £3 | |
| 3.5 mm TRRS sockets (PCB or panel) | 2 | £2 | For the link cable |
| TRRS cable, 30 cm | 1 | £3 | Becomes the link cable via a crossover adapter |
| Perfboard / stripboard, 70×50 mm | 2 | £3 | The button board |
| Silicone hook-up wire, 26–28 AWG | 1 set | £5 | |
| M2 screws and brass heat-set inserts | 1 set | £5 | Case halves |
| Clear acrylic, 1–2 mm, or a printed clear window | 1 | £2 | Screen window |
| PLA or PETG filament | ~80 g | £2 | Printed on the Ender-3 V2 Neo |
| **Build 2 extra** | | **~£50** | |

## Optional extras

| Part | Rough price | For |
|---|---|---|
| LIS3DH accelerometer breakout | £5 | Tilt games |
| Second complete console | ~£65 | Link-cable games (the main reason to build two) |
| USB power meter (inline) | £8 | Measuring battery life and charging |
| Cheap USB gamepad to harvest | £5 | Silicone button membrane |

## Tools

Soldering iron with a fine tip, solder, flush cutters, wire strippers, a
multimeter (for **polarity checks** and current measurement), heat-shrink and a
heat gun (or a lighter, carefully, by an adult), and the 3D printer.
