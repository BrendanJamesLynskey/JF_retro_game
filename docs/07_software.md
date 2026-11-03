# 7. Software

## Two stages

| Stage | Language | Why |
|---|---|---|
| First games | **MicroPython** (Pimoroni's build, with PicoGraphics) | Edit and run in seconds from Thonny, with no compiler. Easy for a beginner to read and change. Fast enough for simple games at 160×120 |
| Later | **C/C++ with the Pico SDK** | Needed for the PIO pixel-doubler, the DMA double-buffering, the three-channel sound mixer and busy games. The game code keeps the same shape, so porting a game is mostly line by line |

Start in MicroPython and move a game to C only when it's too slow. Many games never
will be.

## Folder layout (when code arrives)

```
firmware/
  console/          shared code every game uses
    screen.py       160x120 buffer, palette, doubling, present()
    buttons.py      read, debounce, pressed / held / released
    sound.py        sfx(data), music(pattern), volume
    rumble.py       buzz(ms)
    save.py         high scores and settings in flash
    battery.py      VSYS reading, bars, low-battery sleep
    menu.py         the start-up game picker
  games/
    points_panic/   one folder per game: game.py + config.py
    ...
  main.py           boots straight into menu.py
tools/
  palette.py        makes the 16-colour palette table
  sprites.py        turns small PNGs into byte arrays
```

## The game loop

Every game has the same shape, so they are all easy to read:

```python
import config as C          # every tunable number for this game lives here
from console import screen, buttons, sound, rumble, save

def new_game():
    return {"score": 0, "lives": C.LIVES, ...}

def update(s, b):           # b = this frame's buttons
    ...                     # move things, check hits, change score
    if hit:
        sound.sfx(C.SFX_HIT); rumble.buzz(60); screen.shake(4)

def draw(s):
    screen.clear(C.SKY)
    ...

def run():
    s = new_game()
    while True:
        b = buttons.read()
        if b.held_menu(): return          # Start + Select for 2 s
        update(s, b)
        draw(s)
        screen.present()                  # waits for the next frame slot
```

- **Fixed time step.** `present()` paces the loop to 50 fps. Game speeds are in
  "per frame" units, so a game never runs at different speeds on different builds.
- **`config.py` at the top of each game** holds speeds, gravity, lives, colours and
  sound data, all with comments saying what each number does. Changing a number and
  seeing what happens is the first lesson.
- **No globals across games.** The menu imports a game, calls `run()`, and gets
  control back when it returns.

## The menu

- It appears about half a second after power-on, with a short boot jingle and the
  console's name drawn in pixels.
- It shows a list of games with each one's best score. A D-pad moves through the
  list, and A plays.
- The **Settings** page has brightness, volume, rumble on/off, "reset high scores"
  (hold A for 3 s), and the battery voltage for the curious.

## Saving

- High scores (top five with three-letter initials per game) and settings are saved
  in a small JSON file in the MicroPython file system, or in a reserved flash sector
  in C.
- **Write only when something has changed** (a new high score, leaving settings), to
  avoid wearing out the flash.
- On low battery, save and then show a "plug me in" screen with the backlight dimmed.

## Development set-up

- Thonny on the Linux box or the Pi 5, connected over USB. Saving a file on the
  console runs it straight away.
- For C: the Pico SDK with CMake, with the drag-and-drop UF2 copied over the BOOTSEL
  drive.
- **A desktop runner** (later): a small Pygame wrapper that implements the same
  `console` API, so a game can be tried on the computer at full speed before it is
  copied across. This makes it much faster to change levels.

## Ideas that are easy wins

- **Screen shake**: offset the whole buffer by a few pixels for a few frames.
- **Hit-pause**: freeze the game for 3–4 frames on a big hit. It makes hits feel
  heavy.
- **Particles**: eight little pixels that fly out and fade. One function, used
  everywhere.
- **Attract mode**: if the menu sits idle for 30 s, it plays a recorded demo of a
  game, like an arcade cabinet.
