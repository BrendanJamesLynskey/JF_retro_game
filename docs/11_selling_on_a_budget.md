# 11. Selling it: the cost-down version

The plan is to build a batch and sell them for a small profit. That changes the
design: the build-1 Pimoroni boards make prototyping quick, but they cost too much to
put in every console. This page cuts the parts cost from about £75 to **about £12**,
without losing the fun.

All prices here are **estimates** for a batch of about 20 consoles, including VAT and
a share of the delivery charges. Parts come from AliExpress or LCSC, and the circuit
board from JLCPCB. Check current prices before ordering, because they change a lot.

## The five big savings

| Change | Saves per console | Why it's fine |
|---|---|---|
| **2 × AA rechargeable NiMH cells instead of LiPo + LiPo SHIM** | ~£17 | See the next section. It's also the biggest safety and shipping win |
| **A bare AliExpress 2" ST7789 screen instead of the Display Pack** | ~£15 | It's the same panel. UK shops charge ~£11 for the bare module; AliExpress is ~£3.50–5 |
| **A small printed circuit board (PCB) instead of perfboard and wires** | ~£5 in parts, and an hour of work | Modules solder straight on, with no wiring mistakes. Assembly takes 30 minutes instead of 2 hours |
| **A transistor and small speaker instead of the PAM8302 amp** | ~£3 | Quiet but clear enough for square-wave sound. A volume setting still works |
| **The Pico (RP2040) instead of the Pico 2** | ~£1 | Games draw at 160×120 anyway, and that buffer fits easily in 264 KB. Keep the Pico 2 if a game needs it |

The 3D-printed shell costs about £1 in PLA, so the case is almost free. The real
costs are the screen, the Pico, and the time it takes to build and test each one.

## Batteries: why AA rechargeables beat LiPo for selling

The Pico's power input (VSYS) works from 1.8 V to 5.5 V, so **two AA cells (2.4 V for
NiMH, 3 V for alkaline) run it directly** through its own buck-boost regulator. No
charger chip is needed.

| | LiPo + charger | 2 × AA NiMH |
|---|---|---|
| Parts cost | ~£17.50 (SHIM + cell), or ~£4 with AliExpress charger and cell | ~£0.45 (holder + switch) |
| Energy | 1200 mAh × 3.7 V ≈ 4.4 Wh | 2000 mAh × 2.4 V ≈ 4.8 Wh |
| Play time (estimate) | ~10 h | ~10 h |
| Charging | USB in the console | In any AA charger. Most homes have one, and spare cells swap in as soon as they run flat |
| Safety in a child's hands | A swollen or punctured pouch can catch fire | Very low risk |
| Posting | Royal Mail allows lithium only when fitted in the device, with a battery label and limits per parcel | Ordinary post, with no special rules |
| Retro feel | — | Handhelds of the 1990s ran on AAs |

**Decision for the version we sell: 2 × AA holder on the back, sold without batteries**
(or with two NiMH cells for about £2.50 more). The battery gauge still works: the
Pico reads VSYS on GPIO29, and NiMH runs flat at about 2.0 V for two cells. The
LiPo + USB-C version stays as an optional "deluxe" model.

Caveat: on 2 × AA the screen's 3.3 V supply comes from the Pico's regulator, which can
supply about 300 mA. The screen, Pico and speaker need well under that, but **measure
the current on the prototype**, with the backlight at full brightness and sound
playing, before ordering a batch.

## Cost-down parts list (per console, batch of ~20)

| Part | Rough cost | Notes |
|---|---|---|
| Raspberry Pi Pico (RP2040) | £3.60 | Buy the official one: the clones vary, and some don't measure VSYS on GPIO29 |
| 2.0" 320×240 IPS ST7789 SPI module | £4.00 | AliExpress; buy two spares per ten in case some arrive dead |
| Carrier PCB, 2-layer, ~95 × 70 mm | £0.80 | JLCPCB, about 30 boards; the Pico and screen solder on with headers |
| 8 × 6×6 mm tactile switches | £0.20 | D-pad, A, B, Start, Select |
| 2 × AA holder (PCB mount) + slide switch | £0.45 | |
| 20 mm 8 Ω speaker + S8050 transistor + resistor | £0.50 | |
| Headers, capacitors, resistors | £0.30 | |
| PLA shell, D-pad and buttons (~45 g) | £0.75 | ~£16/kg filament |
| Printer electricity (~4 h) | £0.15 | |
| 4 × M2 screws | £0.10 | Screw into printed holes; no brass inserts needed |
| Box or padded bag, instruction card | £0.60 | The card is printed at home |
| **Parts total** | **~£11.50** | |
| Optional: rumble motor + transistor + diode | +£0.40 | |
| Optional: 2 × NiMH AA cells included | +£2.50 | |
| Optional: link-cable socket (TRRS) | +£0.30 | Makes "buy two" a good sales line |

### Where to buy

Links found on 2026-11-03. AliExpress listings come and go, so search links are
given for the everyday parts.

| Part | Where |
|---|---|
| Raspberry Pi Pico (RP2040) | [The Pi Hut](https://thepihut.com/products/raspberry-pi-pico) |
| 2.0" ST7789 IPS module, on a small PCB with pins (easiest) | [AliExpress 1005007404892390](https://www.aliexpress.com/item/1005007404892390.html) |
| 2.0" ST7789 bare panel, 12-pin solder (cheapest, for the carrier PCB) | [AliExpress 1005009963466356](https://www.aliexpress.com/item/1005009963466356.html) |
| Carrier PCB | [JLCPCB](https://jlcpcb.com/) (upload the KiCad Gerbers) |
| 6×6 mm tactile switches | [LCSC C381012](https://www.lcsc.com/product-detail/C381012.html) (5 mm tall) or [C381017](https://www.lcsc.com/product-detail/C381017.html) (9.5 mm tall, for taller caps) |
| 2 × AA PCB holder | [LCSC C5290182](https://www.lcsc.com/product-detail/Battery-Connectors_MYOUNG-BH-AA-B5BA015_C5290182.html) (SMT; check it's 2-cell) or [LCSC search "2AA holder"](https://www.lcsc.com/search?q=2AA%20battery%20holder) |
| On/off switch | Easiest: a **2 × AA battery box with a built-in switch and lid** ([AliExpress search](https://www.aliexpress.com/w/wholesale-2-aa-battery-box-switch.html)), which replaces both the PCB holder and the slide switch. Otherwise any small SPDT slide switch ([LCSC search "MSK-12C02"](https://www.lcsc.com/search?q=MSK-12C02), ["SK12D07"](https://www.lcsc.com/search?q=SK12D07)) or an 8×8 mm latching push switch ([AliExpress search](https://www.aliexpress.com/w/wholesale-8x8-self-locking-switch.html)) |
| S8050 transistor | [LCSC C8327](https://www.lcsc.com/product-detail/C8327.html) |
| Speaker 8 Ω | [AliExpress search "20mm 8 ohm speaker"](https://www.aliexpress.com/w/wholesale-20mm-8-ohm-speaker.html), or UK: [Pi Hut 20×30 mm cavity speaker](https://thepihut.com/products/20-x-30mm-cavity-speaker-b-with-ph1-25-connector-8-2w) (louder, dearer) |
| Resistors, capacitors, headers | LCSC, in the same order as the switches |
| Coin vibration motor (optional) | [AliExpress search "coin vibration motor 1027"](https://www.aliexpress.com/w/wholesale-coin-vibration-motor-1027.html) |
| 3.5 mm TRRS socket (optional) | [LCSC search "PJ-320A"](https://www.lcsc.com/search?q=PJ-320A) |
| M2 screws | [Amazon UK search "M2 self tapping screws"](https://www.amazon.co.uk/s?k=M2+self+tapping+screws) |
| PLA filament | [Amazon UK search "PLA 1.75mm 1kg"](https://www.amazon.co.uk/s?k=PLA+1.75mm+1kg) |
| NiMH AA cells (optional) | [Amazon UK search "NiMH AA low self discharge"](https://www.amazon.co.uk/s?k=NiMH+AA+low+self+discharge) |
| Padded bags | [Amazon UK search "padded envelopes small"](https://www.amazon.co.uk/s?k=padded+envelopes+small) |

Tip: put all the LCSC parts in **one order** and ship them with the JLCPCB boards
(they are the same group), so the postage is shared.

Other savings:

- **No screen window.** Design the bezel so the screen's own glass sits flush behind an
  opening with a 1 mm lip.
- **Print the button caps in a second colour** using a filament swap at layer height,
  not a second printer. It looks expensive and costs nothing.
- **Printed snap-fit clips instead of screws** are possible but harder to get right.
  Try them on version 2.

## Time per console

| Job | Time |
|---|---|
| Print the shell | ~4 h, unattended; print two at once on the Ender bed |
| Solder the PCB (Pico, screen, switches, holder, speaker) | ~30 min |
| Flash the firmware and run the self-test | ~5 min |
| Assemble the case and pack it | ~10 min |
| **Hands-on time** | **~45 min** |

**Build a self-test into the firmware** (hold Select while switching on): it lights each
button on the screen as it's pressed, plays a tone, checks the battery reading and draws
colour bars. Every console passes it before it's boxed.

## Price and profit (rough)

| Sell price | Fees (Etsy, ~10.5% + ~£0.36) | Parts | Left per console |
|---|---|---|---|
| £25 | £3.00 | £11.50 | **~£10.50** |
| £30 | £3.50 | £11.50 | **~£15** |
| £35 | £4.00 | £11.50 | **~£19.50** |

- Selling in person (a school fair, a craft fair, friends) avoids the fees.
- Postage is charged to the buyer: a small parcel costs about £3–4.
- **Set aside money for failures:** assume 1 in 10 needs a replacement screen or a
  rebuild.
- The first batch also pays one-off costs: PCB set-up, spare screens and test prints.
  Expect about £30–40 before the first profit.
- **Sell the games as much as the hardware.** "Comes with five original games, and more
  to download free" is the selling point. A cheap handheld from AliExpress loaded with
  emulators can't say that.

## Before selling: rules to check

Selling electronics in the UK comes with legal obligations. This isn't legal advice,
but these are the things to look into **before the first sale**:

- **Who sells.** Etsy and eBay sellers must be 18 or over, so an adult runs the shop.
- **Product safety.** Everything sold must be safe, and you need your name and address on
  the product or its packaging.
- **Is it a toy?** A product designed or clearly meant for children under 14 counts as a
  toy and falls under the Toy Safety Regulations. Those require a safety assessment,
  usually testing, and the UKCA mark, which is a big job for a small seller. This is
  another reason to choose AA cells over a LiPo.
- **Electronics rules.** Electronic products also fall under the EMC (interference) and
  RoHS (hazardous substances) rules, which normally mean a declaration of conformity and
  the UKCA mark.
- **Recycling.** Selling electrical goods and batteries can bring WEEE and battery
  take-back duties, which are light for very small sellers.
- **Insurance.** Product liability insurance for a small craft business costs from about
  £100 a year.
- **Where to ask.** The free guides at [Business Companion](https://www.businesscompanion.info/)
  (written for Trading Standards) and your local Trading Standards office, which gives
  advice to small businesses.

A cheaper route while the business is small: **sell it as a kit for older children and
adults to build themselves** (PCB, parts, printed shell and instructions). That is a
genuinely different product with a different customer, and it's fun to build. Kits still
have to be safe and still need the right markings, so check how the rules apply to kits
too.

## Order of work

1. Finish the prototype from [Build plan](10_build_plan.md) stages 0–3 on the Pimoroni
   boards. Games come first: if nobody wants another go, there's nothing to sell.
2. Build one **AA-powered** prototype on perfboard, measure its current and play time,
   and check that the screen looks right on 2.4 V.
3. Design the carrier PCB (KiCad), order five, and build two consoles.
4. Give them to five people to play for a week. Fix what they complain about.
5. Check the rules above. Then order a batch of 20.
