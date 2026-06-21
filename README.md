# Softub Whirlpool Smart Conversion

## Table of Contents

- [Summary](#summary)
- [Motivation](#motivation)
- [Components](#components)
- [Quick Start](#quick-start)
- [Disclaimer & Safety](#disclaimer--safety)

## Summary

Retrofit of a Softub Legend (T-140S/T-220S/T-300S, 230V/1400W) portable whirlpool's dead OEM control board with WiFi smart-home parts — variable-speed pump (Shelly + Kemo), temperature control (Sonoff TH Elite), passive ozone disinfection, and live pH/ORP/TDS water monitoring.

![Softub motor housing exterior](images/01-softub-exterior.jpg)

When the original control board on my Softub whirlpool died, replacement parts were scarce and the design itself was a closed black box. This project replaces it entirely with off-the-shelf smart home components: a Sonoff TH Elite handles temperature-based switching, a Shelly 0-10V dimmer paired with a Kemo M240 power controller gives the pump stepless speed control from a quiet 35% up to full 100% jet mode, a passive ozonator handles disinfection via the original Venturi injector, and a Tuya WiFi water analyzer keeps live pH/ORP/TDS readings without any proprietary app. Fully documented: wiring diagrams, bill of materials, and build guide.

> **Status:** Personal project, documented as I build it. Currently private while I finish testing; will be opened up once the build is stable.

![Wiring diagram](diagrams/wiring-diagram.svg)

*[Download the interactive HTML version](diagrams/wiring-diagram.html) for a richer view with notes — open in any browser.*

## Motivation

![Original Softub topside control panel](images/08-softub-topside-panel.jpg)

The Softub Legend (~10 years old) stopped working: the motor wouldn't start and the JET button on the topside panel showed no reaction. The motor had intermittent starting difficulties before the complete failure.

The official service partner diagnosed a defective control board plus an age-related capacitor and quoted **CHF 1,100** for the repair (negotiated down to CHF 1,000). The capacitor alone was quoted at CHF 99. Balboa Water Group confirmed that the pump (part 1019230, motor 1114033 — MTR USM 1660 1HP 1SP 5.4A HV/50Hz) is a custom unit made exclusively for Softub and is **discontinued** with no modern replacement available.

Rather than paying CHF 1,000+ for a like-for-like repair of a proprietary, non-repairable control board with no remote control and only on/off pump operation, the entire control system was replaced with off-the-shelf smart home components for **under CHF 300**. The original Balboa motor was kept — only the control electronics were replaced. The new system is:

- **Repairable** — every part is a standard, replaceable component, not a proprietary board
- **Remotely controllable** — temperature, pump speed, and water chemistry are all visible and adjustable from a phone
- **Variable-speed** — the pump can run anywhere from a quiet 35% filtration speed up to full 100% jet power, instead of just on/off

## Components

| Function                            | Component                                                   |
| ----------------------------------- | ----------------------------------------------------------- |
| Temperature control / main switch   | Sonoff TH Elite + DS18B20 probe                             |
| Variable pump speed (0–10V control) | Shelly Dimmer 0/1-10V PM Gen3                                    |
| Power stage for the pump motor      | Kemo M240 power controller                                  |
| Ozone disinfection                  | Passive 220V ozonator (original Venturi injector reused)    |
| Water chemistry monitoring          | Tuya W218 (pH / ORP / TDS), temp probe on the Kemo housing  |
| Distribution                        | Terminal block, Wago lever connectors, waterproof gel boxes |

![New smart components wired: TH Elite, Shelly dimmer, Kemo M240](images/18-new-th-elite-wired.jpg)

See [docs/COMPONENTS.md](docs/COMPONENTS.md) for what each part does and why it was chosen, and [bom/bill-of-materials.md](bom/bill-of-materials.md) for the full parts list with sources and prices.

## Quick Start

1. Read the [Disclaimer & Safety](#disclaimer--safety) section below — this project involves 230V mains wiring.
2. Review the [wiring diagram](diagrams/wiring-diagram.html) (open in any browser).
3. Follow [docs/GUIDE.md](docs/GUIDE.md) for the build sequence.
4. Check the [bill of materials](bom/bill-of-materials.md) for exact parts.

## Disclaimer & Safety

> **This project involves 230V AC mains wiring. Incorrect wiring can cause fire, electric shock, or death.**

![Softub rating plate — 230V ~50Hz, 6.0A, 1400W, IPX5 Class 1](images/07-softub-rating-plate.jpg)

This is a personal DIY project by a hobbyist — **not** a licensed electrician and **not** an electrical engineer. It has not been professionally reviewed, certified, or inspected. The wiring, component choices, and design documented here may contain errors. This is not a UL/CE/IEC-certified design.

- **No liability.** The author accepts no responsibility for injury, death, property damage, fire, or any other loss resulting from replicating, adapting, or referencing this project.
- **No guarantee of correctness.** Use this as a reference, not as an instruction manual.
- **Local regulations apply.** Electrical codes vary by country and region. What is described here may not be legal or compliant in your jurisdiction.
- **Insurance and warranty.** DIY modifications to mains-powered appliances may void the manufacturer's warranty and affect your home or liability insurance.
- **Hire a professional.** If you are not confident working with mains voltage, hire a qualified electrician. At minimum, have one inspect your wiring before energizing it.

### Safety Rules Followed in This Build

- Always unplug before touching anything — never just switch off.
- Upstream 30mA RCD/GFCI is mandatory (Type A is sufficient — the Kemo M240 is a phase-angle controller, not a DC-producing VFD).
- Discharge the pump capacitor (CBB60, 450V) before handling.
- **Wire colours verified with a multimeter, never assumed from colour alone.** Components in this build use conflicting regional conventions — the motor uses US colours (black = neutral, white = live) while the ozonator uses the exact opposite. Neither matches EU convention (brown/blue). The original motor wiring was also in poor condition (corroded spade terminals, degraded insulation) and needed repairing before reconnecting.

![Motor wires — US color convention (green=earth, black, white/yellow)](images/10-motor-wires-us-colors.jpg)

![Motor internal terminals — corroded, needed repair](images/11-motor-internal-terminals.jpg)
- Protective earth (PE) connected on every metal enclosure.
- Low-voltage signal wiring physically separated from 230V wiring.
- All connectors inside the motor housing use sealed gel-box connectors (Wago) — condensation is routine, not an edge case.

### If You Replicate This Project

- Have a qualified electrician inspect your wiring before first power-up.
- Test incrementally with a multimeter at each stage, not all at once.
- Keep the whirlpool empty of water during initial dry testing.
