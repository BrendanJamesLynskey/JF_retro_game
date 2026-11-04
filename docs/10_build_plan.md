# 10. Build plan

Each stage ends with **something you can play**. Stop at any stage and it's
still a working toy.

## Stage 0: hello screen (an evening)

- Solder the headers through the Pico and the LiPo SHIM together (the SHIM is hard to add once the headers are on). Leave the cell unplugged for now.
- Flash Pimoroni's MicroPython (the Pico 2 build) onto the Pico 2.
- Plug in the Display Pack and run a demo: text, a moving square and the buttons
  lighting the RGB LED.
- **Done when:** pressing A turns the LED green and moves a square on the screen.

## Stage 1: first game on the desk (a weekend)

- Write the shared `console/` code: the 160×120 buffer, buttons and saving. There's
  no speaker yet, so feedback is the RGB LED, screen flashes and screen shake.
- Build **Points Panic** with four buttons and one layout.
- Add high scores saved to flash.
- **Done when:** someone else plays a full round and wants another go.

## Stage 2: on battery (an evening)

- Plug the cell into the LiPo SHIM (**check polarity first**).
- Add the battery bars to the menu and the low-battery save-and-sleep screen.
- Run the battery-burn test and write down the hours here.
- **Done when:** it plays unplugged, charges from USB, and the measured run time is
  noted in [Power and battery](03_power_and_battery.md).

## Stage 3: sound and rumble (an evening or two)

- Wire up the PAM8302, the speaker and its RC filter on a small piece of perfboard.
- Wire up the rumble motor and transistor.
- Add sound effects and rumble to Points Panic.
- **Done when:** a crash goes *bang* and buzzes in your hands.

## Stage 4: the handheld (two or three weekends)

- Draw the case in OpenSCAD or FreeCAD: front shell, back shell, D-pad cross, button
  caps, screen window, a battery pocket and a speaker box.
- Print test pieces of the **button area first** and try the feel before printing the
  whole case.
- Wire the button board on perfboard following the
  [build 2 pin map](06_pin_map_and_wiring.md), and fit the bare screen.
- Change `buttons.py` to the D-pad pins. Games written for build 1 still work, because
  X/Y map onto the D-pad's left/right.
- **Done when:** it lives in a pocket for a week and nothing comes loose.

## Stage 5: more games (ongoing)

- One new game at a time from [Game ideas](08_game_ideas.md), each finished before the
  next is started.
- Port to C only the games that need it.

## Stage 6: two consoles and the link cable

- Build a second console.
- Make the crossover link cable and write the lockstep link code.
- **Snowball Standoff** is the first link game.
- **Done when:** two people sit at opposite ends of a sofa throwing snowballs at each
  other.

## Things to decide later

- A wide layout (controls either side of the screen) or a narrow one, decided after
  holding printed mock-ups of both.
- A silicone membrane or tactile switches, decided after trying both.
- Whether to make a custom PCB (KiCad) once the hand-wired version has proved the
  design. That would give a thinner case and cleaner assembly, at about £20 for
  five boards.
- A name for the console: chosen, **HORIZON** (check the trade mark before selling).
