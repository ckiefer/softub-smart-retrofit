# Components

What each part does, why it was chosen, and what it replaced.

> All of the active control and sensor components (TH16A, Shelly, Tuya W218) are WiFi-native devices, chosen deliberately so they can later be brought into an existing Home Assistant setup for unified dashboards and automations, rather than being locked into separate single-purpose apps. That integration isn't built yet — see the README's Roadmap section.

## Control & smart home

### Sonoff TH16A
The master switch for the whole system. Reads water temperature via the DS18B20 probe and switches 230V to everything downstream (Shelly/Kemo chain and the ozonator) once a target temperature is reached. Rated 16A, well above what the pump draws.

Replaces: the original Softub control board's temperature logic, which lived on a sealed, non-repairable PCB.

### DS18B20 temperature probe
Waterproof 1-Wire digital temperature sensor, wired into the TH16A's dedicated sensor input. Needs a 4.7kΩ pull-up resistor between data and VCC.

### Shelly Plus 0-10V Dimmer
Generates a 0–10V DC control signal from the Shelly app, which the Kemo M240 (below) interprets as a target pump speed. Configured as a "decoupled switch" (output independent of any physical button input), with output limited to a 35–100% range in the app's brightness settings, and configured to power on at 35% whenever it receives mains power — never at 0%, and never silently resuming whatever percentage it was left at before a power cycle.

The 35% floor exists because the pump's single-phase induction motor has a minimum breakaway torque requirement: below roughly 3–3.5V (30–35%) the motor doesn't have enough force to start turning and will just hum and heat up, which is genuinely bad for the windings if sustained.

### Kemo M240
A small German-made AC power controller. Takes 230V in on one side, and a 0–10V DC signal on a galvanically isolated control side, and outputs a proportional AC voltage to the pump. This is what makes the pump variable-speed instead of just on/off.

**An earlier design (abandoned)** tried to add a hardware bypass relay so the pump could run at true full power without the Kemo carrying the full load continuously (to keep the controller cool). That used a Sonoff Mini R2 wired as a changeover switch upstream of the Kemo's mains input — when off, current passed through to the Kemo as normal; when on, it cut the Kemo's supply and routed mains directly to the pump instead. It worked in principle but needed a separate external relay (the Sonoff Mini's own relay isn't wired as a changeover/NC-NO pair usable this way) and added real complexity for a quality-of-life feature. The final build dropped it: the Shelly dimmer's range was simply extended to 35–100%, and the Kemo carries full load occasionally during jet mode. Its housing does get noticeably warmer at 100% than at typical filter-mode loads — worth checking by hand occasionally during long full-power sessions.

## Pump & motor

### Original Balboa motor
Kept from the original installation. Single-phase induction motor with a separate run capacitor terminal. US-style wire colour convention (black = neutral, white = live on this particular unit — verified by resistance measurement, not assumed, since this is the opposite of EU convention and also opposite of the colour convention used by the ozonator, on the same build).

### CBB60 run capacitor (20µF, 450VAC)
Replacement for the original (failed) capacitor. Has two pairs of spade terminals (each pair electrically identical, just doubled for easier wiring) — only one terminal per pair is used; the spare is insulated, not left bare.

## Water treatment

### Ozonator (passive, 220V)
A simple corona-discharge ozone generator with no internal air pump. It cannot push ozone into the water by itself — it relies entirely on the Venturi effect created by water flowing past a constriction in the plumbing, which draws a vacuum and pulls the ozone gas in. This is why the ozonator is wired in parallel with the pump's switched output rather than to a permanently-on circuit: running it while the pump is stopped does nothing (no vacuum, no flow), so there's no point giving it independent control.

A check valve sits in the air line between the ozonator and the water connection, oriented to allow air/ozone to flow toward the water but block water from flowing back toward the ozonator.

### Tuya W218 water analyzer
An 8-in-1 WiFi water quality monitor with probes for pH, ORP, and TDS that sit directly in the pool water, plus a temperature probe input. Wired independently off the terminal block (its own 230V tap, not switched by the TH16A), so it keeps reporting water chemistry continuously, even between heating/filter cycles.

Its temperature probe is **not** in the water — it's mounted on the Kemo M240's housing instead, purely out of curiosity about how warm the controller actually gets under load. Worth knowing if you're reading this device's "temperature" reading and expecting it to mean water temperature: it doesn't, in this build.

## Distribution & wiring

### Terminal block
Central L/N/PE distribution point. Everything downstream branches from here rather than daisy-chaining off the mains plug directly.

### Wago 221 lever connectors + Wago gel boxes
221-series lever connectors for dry, accessible junctions; sealed gel-filled boxes (Raytech Gel Box) for any joint inside the motor housing, which sees condensation and occasional splash. A standard twist-and-tape joint is not acceptable in that environment.

### Single-core wire (brown/blue/green-yellow) + heat shrink + ferrules + spade connectors
Standard EU colour-coded wiring for extensions and repairs, finished with insulated ferrules on stranded ends going into screw terminals, and insulated 6.3mm spade (Faston) connectors with heat shrink over any exposed metal, particularly on the capacitor terminals.

## Backup / not installed

### Sonoff Basic ×2 + Sonoff WTS01 waterproof temperature sensor ×2
Purchased as a like-for-like spare in case the TH16A fails and a replacement is needed quickly. **Not wired into the system.** There was no room for an additional enclosure inside the motor housing, so these are kept in storage rather than mounted.

## Tools used

- UNI-T UT133A digital multimeter — continuity/resistance testing (essential for verifying wire colour conventions on imported parts before connecting anything)
- JX-1601 crimping tool with interchangeable dies — die 08 (0.25–1.0mm²) for thin sensor cable, die 10 (0.5–1.5mm²) for capacitor spade connectors
- Wire stripper (adjustable, 0.5–6mm²) — for stripping individual conductor ends before crimping or screw-terminal connections
- Cable jacket knife — for slitting the outer sheath off multi-core mains cable without nicking the conductors inside; run flat along the cable rather than pressed in perpendicular
- Soldering iron + solder — for tinning stranded wire ends where a crimp connector wasn't practical, and for the BNC cable extension joins (centre conductor and shield soldered separately, insulated from each other with heat shrink before a final outer layer)
