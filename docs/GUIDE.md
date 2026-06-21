# Build Guide

A walkthrough of the conversion in the order it was actually done. Read [`SAFETY.md`](SAFETY.md) first.

## 1. Remove the original electronics

Unplug the spa. Wait at least 5 minutes and discharge any capacitors before touching anything.

Photograph and label every wire before disconnecting it — the original board's labelling (relay, transformer, valve) is not obvious once it's in pieces. Remove:

- The main control PCB (Softub Inc., marked "C-2013" on the unit used here)
- The Zettler relay (rated 220V coil, 30A/277VAC contacts — this was the switching element for the pump)
- The Isonic solenoid valve (12VDC, normally-closed, used in the original design as a Venturi-effect ozone injector — see [`COMPONENTS.md`](COMPONENTS.md) for why this turned out to be unnecessary in the new design)
- The Aquatemp transformer (230V primary, 12V secondary — only powered the control logic, not the motor)

None of these are reused.

## 2. Identify the motor wiring

The motor has four leads: live, neutral, earth, and a capacitor tap. **Do not assume colours match EU convention** — many components in this build (motor, ozonator) use US/Asian colour conventions where black is sometimes neutral and white is sometimes live, and the two devices in this particular build used *opposite* conventions from each other.

Verify with a multimeter before connecting anything:
1. Set the meter to continuity/resistance mode.
2. With the device unplugged, measure resistance between each pair of leads.
3. Two leads will show a resistance value (these are the winding — live and neutral). One will show open circuit / no continuity to the others (this is the capacitor tap, or earth if the housing is metal and earth is bonded to it).
4. For polarity (which is live vs. neutral), check the printed motor nameplate if available, or treat both as electrically equivalent if the motor is a simple single-phase capacitor-start type — for this kind of motor, swapping live/neutral simply reverses rotation direction, with no damage risk. Run it briefly without the capacitor connected first (it will hum but not spin) to confirm it's electrically intact before wiring the capacitor.

## 3. Mount the terminal block

Install on a stable bracket or mounting plate inside the housing, positioned so all downstream components can reach it with reasonably short runs. This is the single point all new wiring radiates from — getting its position right first makes everything after it easier.

## 4. Wire the capacitor

- Confirm capacitance and voltage rating against the motor's original capacitor or nameplate spec (this build used 20µF / 450VAC as a replacement).
- Crimp insulated 6.3mm spade connectors onto the motor and capacitor leads — see [`COMPONENTS.md`](COMPONENTS.md) for crimping tool/die notes.
- The capacitor has two electrically-identical spade terminal pairs per side; use one per side and insulate the unused spare with heat shrink, since it sits exposed inside a housing that does see condensation.

## 5. Wire the TH16A

- Terminal block L/N → TH16A IN-L/IN-N
- TH16A OUT-L/OUT-N → downstream to the Shelly/Kemo chain and the ozonator (see wiring diagram)
- DS18B20 → TH16A's dedicated sensor input, with a 4.7kΩ pull-up resistor between data and VCC
- Mount the DS18B20 probe somewhere it reads actual water temperature, not trapped air

## 6. Wire the Shelly Plus 0-10V Dimmer

- TH16A OUT-L/OUT-N → Shelly L/N (the Shelly needs its own mains supply to be able to output a signal at all — this is easy to miss, since the app will happily show a percentage value even with zero volts actually present on the output if the device itself isn't powered)
- Shelly 0–10V output (+ / –) → Kemo's 0–10V signal input
- In the Shelly app:
  - Set the input type to **"Detached switch"** (decoupled from the relay) — the default "single-button dimming" mode expects a physical button on S1/S2 and may not output correctly without one
  - Set min/max brightness to **35% / 100%**
  - Set "action on power-on" to **"turns on when powered"**, not "restore last state" — this guarantees it always starts at the 35% floor rather than potentially resuming whatever value it was left at

## 7. Wire the Kemo M240

- Shelly's 0–10V output → Kemo's "0-10V/DC" input terminals (the small left-hand pair — easy to overlook on first glance, since most of the visible wiring on this device sits on the right-hand 230V side)
- TH16A's switched output → Kemo "INPUT 230VAC"
- Kemo "OUTPUT LOAD" → pump motor live
- Pump motor neutral → terminal block neutral (shared with everything else)

## 8. Wire the ozonator

- Identify live/neutral/earth on the ozonator's cable using a multimeter (do not assume colour convention — see step 2's warning, which applies here too).
- Wire it electrically in parallel with the Kemo's output to the pump, so it only receives power while the pump is actually running.
- Connect the air-side tubing: ozonator output → check valve (oriented away from the ozonator) → Venturi injector fitted into the water line.
- This ozonator has no internal air pump — it cannot push ozone into still water. If it's tested with the pump off and "nothing seems to happen," that's expected: test by holding the open end of the air tubing under water with the pump running.

## 9. Wire the Tuya W218 water analyzer

- Tap power directly from the terminal block (own L/N leads), independent of the TH16A switched circuit, so it stays on even when the pump cycle is off.
- pH, ORP, and TDS probes go into the pool water.
- The temperature probe in this build does **not** go into the water — see [`COMPONENTS.md`](COMPONENTS.md) for why it's mounted on the Kemo housing instead. Mount it wherever you actually want it measuring.

## 10. Final assembly

- Route all 230V wiring and all low-voltage signal/sensor wiring with as much physical separation as the enclosure allows.
- Every junction inside the housing goes through a sealed gel-box connector (Wago) — condensation inside this enclosure is routine, not an edge case.
- Pack foam/padding material where needed to keep components from rattling against the housing or each other — offcuts work fine, this doesn't need to be pretty.

## 11. Test before refilling

1. With the spa still empty of water, plug in and verify the TH16A powers on and its display/app shows correctly.
2. Check the Shelly app connects and reports status.
3. Set the Shelly to 35% and confirm — with a multimeter — that roughly 3.5V DC appears across the Kemo's signal input terminals.
4. Confirm the Kemo's output terminals show a proportional AC voltage.
5. Briefly run the pump dry (a few seconds only) to confirm rotation and that nothing smells hot or sounds wrong, then stop before any heat buildup.
6. Refill, then re-test the full cycle with water present, checking actual water flow at low (35%) and high (100%) settings, and confirming ozone bubbles appear at the Venturi injection point while the pump runs.

See [`TROUBLESHOOTING.md`](TROUBLESHOOTING.md) for problems encountered during this exact sequence and how they were diagnosed.
