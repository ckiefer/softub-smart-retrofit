# Softub Smart Conversion

*Retrofit of a Softub portable spa's dead OEM control board with WiFi smart-home parts — variable-speed pump (Shelly + Kemo), temperature control (Sonoff TH16A), passive ozone disinfection, and live pH/ORP/TDS water monitoring.*

When the original control board on my Softub spa died, replacement parts were scarce and the design itself was a closed black box. This project replaces it entirely with off-the-shelf smart home components: a Sonoff TH16A handles temperature-based switching, a Shelly 0-10V dimmer paired with a Kemo M240 power controller gives the pump stepless speed control from a quiet 35% up to full 100% jet mode, a passive ozonator handles disinfection via a Venturi injector, and a Tuya WiFi water analyzer keeps live pH/ORP/TDS readings without any proprietary app. Fully documented: wiring diagrams, bill of materials, build guide, and a troubleshooting log of every real issue hit along the way.

> **Status:** Personal project, documented as I build it. Currently private while I finish testing; will be opened up once the build is stable.

📐 [**View the full wiring diagram**](diagrams/wiring-diagram.html) · [**View the before/after overview**](diagrams/conversion-overview.html)
*(Both are interactive HTML files — download or clone the repo and open them in a browser; GitHub's file preview also renders them reasonably well.)*

## Why

The original Softub control board (a sealed unit with a fixed relay, solenoid valve, and small transformer) is hard to repair, offers no remote control, and gives only on/off pump control. This project replaces it with off-the-shelf smart home parts that are:

- **Repairable** — every part is a standard, replaceable component, not a proprietary board
- **Remotely controllable** — temperature, pump speed, and water chemistry are all visible and adjustable from a phone
- **Variable-speed** — the pump can run anywhere from a quiet 35% filtration speed up to full 100% jet power, instead of just on/off

## What's inside

| Function | Component |
|---|---|
| Temperature control / main switch | Sonoff TH16A + DS18B20 probe |
| Variable pump speed (0–10V control) | Shelly Plus 0-10V Dimmer |
| Power stage for the pump motor | Kemo M240 power controller |
| Ozone disinfection | Passive 220V ozonator + venturi injector |
| Water chemistry monitoring | Tuya W218 (pH / ORP / TDS), temp probe on the Kemo housing |
| Distribution | Terminal block, Wago lever connectors, waterproof gel boxes |

See [`docs/COMPONENTS.md`](docs/COMPONENTS.md) for what each part does and why it was chosen, and [`bom/bill-of-materials.xlsx`](bom/bill-of-materials.xlsx) for the full parts list with sources and prices.

## Repository structure

```
softub-smart-conversion/
├── README.md                   ← you are here
├── docs/
│   ├── GUIDE.md                 ← step-by-step build guide
│   ├── COMPONENTS.md            ← what each component does
│   ├── SAFETY.md                ← electrical safety notes (read first!)
│   └── TROUBLESHOOTING.md       ← problems encountered and fixes
├── diagrams/
│   ├── wiring-diagram.html      ← full wiring schematic (Rev 6)
│   └── conversion-overview.html ← before/after visual overview
└── bom/
    └── bill-of-materials.xlsx   ← parts list with sources & prices
```

## Quick start

1. Read [`docs/SAFETY.md`](docs/SAFETY.md) — this project involves 230V mains wiring.
2. Review the [wiring diagram](diagrams/wiring-diagram.html) (open in any browser).
3. Follow [`docs/GUIDE.md`](docs/GUIDE.md) for the build sequence.
4. Check the [bill of materials](bom/bill-of-materials.xlsx) for exact parts.

## Roadmap

- [ ] Integrate all WiFi components (TH16A, Shelly, Tuya W218) into an existing Home Assistant setup for unified dashboards, automations (e.g. scheduled ozone cycles, temperature-based notifications), and history/logging beyond what each device's own app provides.
- [ ] Document the Home Assistant integration once it's built (entities, automations, any custom integration needed for the Tuya W218).

## Disclaimer

This documents a personal DIY project. It is **not** a certified or professionally reviewed electrical installation guide. Mains wiring carries real risk of injury or death if done incorrectly. If you're not comfortable working with 230V AC, hire a qualified electrician — at minimum, have one inspect your work before energizing it. See [`docs/SAFETY.md`](docs/SAFETY.md) for details.

## License

All rights reserved (for now). This will move to an open license once the build is finalized and published publicly.
