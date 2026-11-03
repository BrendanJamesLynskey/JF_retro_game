# 5. Sound, rumble and extras

## Sound

- **Amp:** a PAM8302 mono class-D breakout (2.5 W max; we will use far less).
  It's powered from VSYS, its input comes from a PWM pin through a simple RC
  low-pass filter (1 kΩ + 10 nF), and its SD (shutdown) pin goes to a GPIO so it's
  fully off when the game is silent.
- **Speaker:** a 28 mm or 20 mm 8 Ω speaker, glued to a printed grille with a small
  sealed back box. The back box makes a surprising difference to loudness.
- **A volume setting** in the menu. Mute is a volume, too.
- **Headphones** (optional): a 3.5 mm jack with a switched contact that cuts the
  speaker when a plug goes in.

### Making the sounds

Retro sound is part of the fun, and it's cheap to make:

- **Two square-wave channels plus a noise channel**, mixed in software and sent out
  as 8-bit PWM samples at about 22 kHz. A DMA channel streams the samples, the
  second core keeps the buffer topped up, and the first core runs the game.
- Sound effects are short lists such as `[(freq, ms), ...]` with a pitch slide. Jump,
  coin, hit and game over are written as data, so they're easy to change.
- **Music:** a tiny tracker format of notes per row in a text string. Each game has
  one loop of eight to sixteen bars.
- In MicroPython, start simpler: one channel using `PWM.freq()` for sound effects
  only. Add the mixer when the games move to C.

## Rumble

- A **coin-type vibration motor** (10 mm, 3 V) stuck to the back shell, near where
  the hands grip.
- It's switched by an NPN transistor (2N2222 or similar) or a small N-MOSFET from a
  GPIO, with a flyback diode across the motor and a 1 kΩ base resistor.
- Use **short buzzes only** (30–120 ms) for a crash, catching something or losing
  a life. Constant rumble gets annoying and drains the battery.
- Rumble can be turned off in the menu.

## Link cable (two-player)

This is the "show off" feature, and probably the most fun extra per pound spent.

- A **3.5 mm TRRS socket** on each console carries UART TX, UART RX, GND and a "cable
  present" sense line.
- The cable is a TRRS cable that **crosses TX and RX**. An off-the-shelf cable won't
  do this, so make a short adapter, or wire the sockets crossed on one console's
  "host" variant. Label it clearly.
- Run at 115200 baud. Each frame, each console sends a few bytes (buttons, frame
  number, a checksum). Both consoles run the same game step from the same inputs:
  the lockstep approach, the same idea old link-cable games used.
- Games that suit it: head-to-head duels, racing, and co-op where each player sees a
  different half of the world.
- Fit the 3.3 V UART lines with 1 kΩ series resistors so that plugging in a wrong
  cable can't damage anything.

## Optional extras (later)

| Extra | Part | Why it's fun |
|---|---|---|
| Tilt control | LIS3DH accelerometer on I²C | Marble and balance games, "shake to roll the dice" |
| RGB status LED | one small 5 mm RGB LED or a WS2812 | Glows the player's colour in link games and pulses on low battery |
| Clock | use the RP2350's timer plus a saved time, or a small RTC | "Daily challenge" games that change each day |
| Light sensor | a phototransistor on an ADC pin | Auto-brightness saves battery; also a "hide in the shadows" game |
| Cartridges (just for fun) | a small I²C EEPROM on a printed cart with an edge connector | A physical cartridge holding saved levels or a game's data. Very retro, and entirely optional |
