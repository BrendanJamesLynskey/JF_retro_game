# 8. Game ideas

All original, and each designed for this console: 160×120 pixels, a D-pad, two
buttons, rumble and one- to three-minute rounds. The first games need only the four
buttons on the build 1 Display Pack.

## Launch title: Points Panic

*You're the signaller. The trains don't stop.*

**Screen:** a small railway layout seen from above that fills the screen, with
coloured stations round the edge, one way in from the left, and four to six sets of
points.

**Core loop:**

- Trains arrive from the entrance, each painted the colour of the station it wants.
- You move a cursor between the sets of points (D-pad, or X/Y on build 1) and flip
  them (A). Each one shows which way it's set with a little arrow.
- A train that reaches the station of its own colour earns points, a "ding" and a
  green flash. A train at the wrong station costs a life, a "buzz" and a rumble.
- Two trains on the same piece of track crash: a big screen shake, a cloud of
  pixels, and two lives lost.
- B = **hold the next train** at the entrance signal for up to three seconds. It has
  a short cooldown.

**Why it's fun:** it starts calm and builds into frantic juggling. Every mistake is
your own. A round lasts about two minutes, so "one more go" is easy.

**Difficulty curve:** a new colour every 30 seconds, trains a little faster each
minute, and after 2 minutes an express train (it flashes and can't be held).

**Tunables in `config.py`:** `TRAIN_SPEED`, `SPEED_UP_PER_MIN`, `SPAWN_GAP`,
`HOLD_TIME`, `LIVES`, the layout as an ASCII map (so new layouts are easy to draw).

**Link-cable mode (later):** two signallers share one layout, each controlling half
of the points. You must talk to each other.

## More ideas

| Game | One-line pitch | Buttons | Why it suits this console |
|---|---|---|---|
| **Kite Day** | Fly a kite through gusts and over trees, using one button to pull the line in and letting go to rise | A only | One button, instantly understood, and rumble on every gust |
| **Mole Post** | Dig tunnels to deliver letters to underground houses before your lamp runs out; old tunnels collapse behind you | D-pad + A (dig faster) | A simple grid, and a new random map each round |
| **Lighthouse Keeper** | Turn the beam to guide boats into the harbour and away from the rocks in a storm; fog hides the boats until the beam hits them | D-pad (turn), A (flash the foghorn) | It looks great in a 16-colour palette with dark sea and a bright beam |
| **Snowball Standoff** | Two players, one console each over the link cable: hide behind snow walls, make snowballs, throw | D-pad + A/B | Shows off the link cable, and the rounds are 60 seconds |
| **Pocket Garden** | A slow, cosy garden that grows a little each real day: water, plant, swap seeds over the link cable | menus | The opposite of the fast games; it uses the saved clock and rewards coming back |
| **Tilt Maze** | Roll a marble through a maze by tilting the console | tilt (accelerometer) | A reason to fit the tilt sensor |
| **Night Bus** | Drive a bus round a tiny town at night, picking up passengers in order, without hitting the cats | D-pad + A (stop) | Top-down driving, with headlight cones as the main graphic |

## Rules for every game

- You're playing within **3 seconds** of choosing it. Any instructions fit on one
  screen.
- It has **a score and a best score**.
- It has **at least three "juice" effects** (sound, shake, particles, rumble, flash).
- **It can be paused** (Start), and when it's paused it says how to quit.
- **Its tunables are in `config.py`.**
- **It is finished before the next one starts.** A small finished game is worth more
  than three big half-made ones.
