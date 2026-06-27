# App Settings & Configuration

## Table of Contents

- [Sonoff TH Elite](#sonoff-th-elite)
- [Home Assistant Integration](#home-assistant-integration)

Software-side configuration for each smart component. This guide covers the initial app setup required after wiring.

## Sonoff TH Elite

In the eWeLink app:

- Set the target temperature to the desired water temperature (e.g. 38°C)
- Set the mode to **"Heat"** — the relay closes when the water temperature drops below the target and opens when it reaches it
- Set the temperature correction/offset if the DS18B20 reading doesn't match a reference thermometer
- Enable **"Power-on state: OFF"** so the TH Elite resumes heating automatically after a power outage rather than staying off until manually restarted

## Home Assistant Integration

This section will be written once the Home Assistant integration is built and tested. Planned topics: entity setup, dashboards, automations (e.g. scheduled ozone cycles, temperature-based notifications), and any custom integration needed for the Tuya W218.
