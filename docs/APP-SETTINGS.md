# App Settings & Configuration

Software-side configuration for each smart component. This guide covers the initial app setup required after wiring.

## Sonoff TH Elite

In the eWeLink app:

- Set the target temperature to the desired water temperature (e.g. 38°C)
- Set the mode to **"Heat"** — the relay closes when the water temperature drops below the target and opens when it reaches it
- Set the temperature correction/offset if the DS18B20 reading doesn't match a reference thermometer
- Enable **"Power-on state: OFF"** so the TH Elite resumes heating automatically after a power outage rather than staying off until manually restarted

## Shelly Dimmer 0/1-10V PM Gen3

In the Shelly app:

- Set the input type to **"Detached switch"** (decoupled from the relay) — the default "single-button dimming" mode expects a physical button on S1/S2 and may not output correctly without one
- Set min/max brightness to **35% / 100%**
- Set "action on power-on" to **"turns on when powered"**, not "restore last state" — this guarantees it always starts at the 35% floor rather than potentially resuming whatever value it was left at

## Home Assistant Integration

This section will be written once the Home Assistant integration is built and tested. Planned topics: entity setup, dashboards, automations (e.g. scheduled ozone cycles, temperature-based notifications), and any custom integration needed for the Tuya W218.
