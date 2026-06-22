# Components

What each part does, why it was chosen, and what it replaced.

> All of the active control and sensor components (TH Elite, Shelly, Tuya W218) are WiFi-native devices, chosen deliberately so they can later be brought into an existing Home Assistant setup for unified dashboards and automations, rather than being locked into separate single-purpose apps.

## Control & Smart Home

### Sonoff TH Elite
The master switch for the whole system. Reads water temperature via the DS18B20 probe and switches 230V to everything downstream (Shelly/Kemo chain and the ozonator) once a target temperature is reached. Rated 16A, well above what the pump draws.

Replaces: the original Softub control board's temperature logic, which lived on a sealed, non-repairable PCB.

### DS18B20 Temperature Probe
Waterproof 1-Wire digital temperature sensor, wired into the TH Elite's dedicated sensor input. Needs a 4.7kΩ pull-up resistor between data and VCC.

### Shelly Dimmer 0/1-10V PM Gen3
Generates a 0–10V DC control signal from the Shelly app, which the Kemo M240 (below) interprets as a target pump speed. Configured as a "decoupled switch" (output independent of any physical button input), with output limited to a 35–100% range in the app's brightness settings, and configured to power on at 35% whenever it receives mains power — never at 0%, and never silently resuming whatever percentage it was left at before a power cycle.

The 35% floor exists because the pump's single-phase induction motor has a minimum breakaway torque requirement: below roughly 3–3.5V (30–35%) the motor doesn't have enough force to start turning and will just hum and heat up, which is genuinely bad for the windings if sustained.

### Kemo M240

A small German-made AC power controller. Takes 230V in on one side, and a 0–10V DC signal on a galvanically isolated control side, and outputs a proportional AC voltage to the pump. This is what makes the pump variable-speed instead of just on/off.

## Pump & Motor

### Original Balboa Motor

![Balboa motor label](../images/09-balboa-motor-label.jpg)

Kept from the original installation. Custom pump made by Balboa Water Group (GG Industries) exclusively for Softub — confirmed discontinued by Balboa with no modern replacement available.

- **Pump:** 1019230 — UL USM 1HP 1SP 16FT HV/50Hz SOFTUB
- **Motor:** 1114033 — MTR USM 1660 1HP 1SP 5.4A HV/50Hz

Single-phase induction motor with a separate run capacitor terminal. US-style wire colour convention (black = neutral, white = live on this particular unit — verified by resistance measurement, not assumed, since this is the opposite of EU convention and also opposite of the colour convention used by the ozonator, on the same build).

### CBB60 Run Capacitor (20µF, 450VAC)

<!-- TODO: add photo of new CBB60 replacement capacitor with crimped spade connectors -->

Replacement for the original (failed) capacitor. Has two pairs of spade terminals (each pair electrically identical, just doubled for easier wiring) — only one terminal per pair is used; the spare is insulated, not left bare.

## Water Treatment

### Ozonator (Passive, 220V)
Replaces the original AquaSunOzone XL-30 (12V DC, which required the now-removed Aquatemp transformer to run). The new unit is a simple 220V corona-discharge ozone generator with no internal air pump. It cannot push ozone into the water by itself — it relies entirely on the Venturi effect from the original Softub plumbing (Venturi injector and check valve are both reused from the factory installation), which draws a vacuum and pulls the ozone gas in when the pump is running. This is why the ozonator is wired in parallel with the pump's switched output rather than to a permanently-on circuit: running it while the pump is stopped does nothing (no vacuum, no flow), so there's no point giving it independent control.

### Tuya W218 Water Analyzer

<!-- TODO: add photo of Tuya W218 controller unit and probes in water -->

An 8-in-1 WiFi water quality monitor with probes for pH, ORP, and TDS that sit directly in the pool water, plus a temperature probe input. Wired independently off the terminal block (its own 230V tap, not switched by the TH Elite), so it keeps reporting water chemistry continuously, even between heating/filter cycles.

Its temperature probe is **not** in the water — it's mounted on the Kemo M240's housing instead, purely out of curiosity about how warm the controller actually gets under load. Worth knowing if you're reading this device's "temperature" reading and expecting it to mean water temperature: it doesn't, in this build.

## Distribution & Wiring

### Terminal Block
Central L/N/PE distribution point. Everything downstream branches from here rather than daisy-chaining off the mains plug directly.

### Wago 221 Lever Connectors + Wago Gel Boxes

<!-- TODO: add photo of sealed Wago gel box connectors -->

221-series lever connectors for dry, accessible junctions; sealed gel-filled boxes (Raytech Gel Box) for any joint inside the motor housing, which sees condensation and occasional splash. A standard twist-and-tape joint is not acceptable in that environment.

### Single-Core Wire (Brown/Blue/Green-Yellow) + Heat Shrink + Ferrules + Spade Connectors

<!-- TODO: add close-up photo of crimping work (ferrules, spade connectors with heat shrink) -->

Standard EU colour-coded wiring for extensions and repairs, finished with insulated ferrules on stranded ends going into screw terminals, and insulated 6.3mm spade (Faston) connectors with heat shrink over any exposed metal, particularly on the capacitor terminals.

## Tools Used

- UNI-T UT133A digital multimeter — continuity/resistance testing (essential for verifying wire colour conventions on imported parts before connecting anything)
- JX-1601 crimping tool with interchangeable dies — die 08 (0.25–1.0mm²) for thin sensor cable, die 10 (0.5–1.5mm²) for capacitor spade connectors
- Wire stripper (adjustable, 0.5–6mm²) — for stripping individual conductor ends before crimping or screw-terminal connections
- Cable jacket knife — for slitting the outer sheath off multi-core mains cable without nicking the conductors inside; run flat along the cable rather than pressed in perpendicular
- Soldering iron + solder — for tinning stranded wire ends where a crimp connector wasn't practical, and for the BNC cable extension joins (centre conductor and shield soldered separately, insulated from each other with heat shrink before a final outer layer)
