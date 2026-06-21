# Safety

Read this before touching anything in this repository with real tools.

## This involves mains voltage

This project wires 230V AC mains power inside a spa enclosure — an environment with water, condensation, and limited space. Mistakes here can cause fire, electric shock, or death. This is not a beginner electronics project.

**If you are not confident reading a wiring diagram and safely working with mains voltage, stop and hire a qualified electrician.** Use this repository as a reference for them, not as a substitute.

## Non-negotiable rules followed in this build

- **Unplug before touching anything.** All work happens with the mains plug physically disconnected, never just switched off.
- **Upstream RCD/GFCI is mandatory.** A 30mA residual current device must sit between the wall outlet and this entire circuit. This project assumes one is already present in the household wiring; if it isn't, install one before proceeding.
- **Capacitors store charge after power-off.** The pump's start capacitor (CBB60, 450V) can hold a dangerous charge even after unplugging. Discharge it safely (e.g. through a resistor) before handling.
- **All conductors are colour-coded and verified, not assumed.** Wire colour conventions differ between regions (EU vs. US/Asian-manufactured components) — every imported component's wiring was measured with a multimeter before connecting, not guessed from colour alone. See [`TROUBLESHOOTING.md`](TROUBLESHOOTING.md) for examples.
- **Protective earth (PE) is connected on every metal enclosure and motor housing**, even on components where it seemed redundant.
- **All low-voltage signal wiring (0–10V control, sensor cables) is kept physically separated from 230V wiring** wherever the enclosure allows it, to avoid accidental cross-contact.
- **Waterproofing is not optional.** Every connector inside the motor housing (a damp, condensation-prone space) uses sealed/gel-filled connectors (Wago gel boxes) or heat shrink, not bare twisted wire or open-air terminal blocks.
- **No component runs both as a controller and a load-bearing switch above its rating.** The Kemo M240 controller is fused (10A); the Sonoff TH16A is rated for 16A and used as the master switch upstream of everything else, not as a fine-grained dimmer.

## Specific hazards relevant to this build

| Hazard | Mitigation used here |
|---|---|
| Capacitor residual charge | Discharge before handling; never touch terminals directly after unplugging |
| Wrong wire colour assumptions (imported parts) | Multimeter continuity/resistance testing before any connection — see [`TROUBLESHOOTING.md`](TROUBLESHOOTING.md) |
| Water ingress into electrical connections | Sealed gel-box connectors, IP-rated enclosures where space allows, conformal sealant on exposed joints |
| Running the pump motor without its capacitor connected | Tested only briefly (a few seconds) without water/load when isolating faults — extended dry-running without a capacitor causes the motor to overheat |
| Two power sources feeding the pump simultaneously | Architecture intentionally avoided in the final design — see the note on the abandoned hardware-interlock bypass design in [`GUIDE.md`](GUIDE.md) |
| Ozone exposure | Ozonator is wired to run only while the pump circulates water (not independently switched), and uses a check valve to prevent water backflow into the air line |

## What this repo does *not* certify

- This is not a UL/CE/IEC-certified design.
- It has not been reviewed by a licensed electrician.
- Local electrical codes vary — what's described here may not be legal or compliant in your jurisdiction. Check before replicating.
- The author is not an electrical engineer; this is a documented hobbyist project, built with care but without professional sign-off.

## If you replicate this project

- Get a second pair of eyes (ideally a qualified electrician) on your wiring before first power-up.
- Test incrementally: verify voltages and signal levels at each stage with a multimeter before connecting the next component, rather than wiring everything at once and powering on cold.
- Keep the spa empty of water during initial dry testing.
