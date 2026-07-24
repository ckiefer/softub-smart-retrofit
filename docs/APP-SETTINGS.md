# App Settings & Configuration

## Table of Contents

- [Sonoff TH Elite](#sonoff-th-elite)
- [Home Assistant Integration](#home-assistant-integration)

Software-side configuration for each smart component. This guide covers the initial app setup required after wiring.

## Sonoff TH Elite

In the eWeLink app:

- Set the temperature correction/offset if the DS18B20 reading doesn't match a reference thermometer
- Enable **"Power-on state: OFF"** so the TH Elite resumes heating automatically after a power outage rather than staying off until manually restarted

> **Superseded:** the target-temperature/"Heat" mode instructions that used to live here have been replaced by a Home Assistant automation (see below) — the on-device **Auto Mode must stay OFF** in eWeLink/HA, otherwise both systems fight over the same relay. Everything temperature-related now happens in HA.

## Home Assistant Integration

Water temperature, water quality, and heating control all run through Home Assistant now, not the eWeLink app's own logic. Full technical detail (entity IDs, the Tuya datapoint map, rebuild-from-scratch steps) lives in the private `home-assistant-setup` repo's [`docs/whirlpool-water-quality.md`](https://github.com/ckiefer/home-assistant-setup/blob/main/docs/whirlpool-water-quality.md) — this section is the summary for readers of *this* repo.

### Why a custom integration was needed for the Tuya W218

Home Assistant's official cloud `tuya` integration only ever exposed one entity for the W218: its cabinet-temperature probe (not water temperature). Tuya's own device-model API confirms the manufacturer never registered pH/ORP/TDS/etc. in this device's cloud schema at all — those datapoints are only readable over the *local* network protocol, which the standard `localtuya` HACS integration also can't use here (it tops out at protocol 3.4; this device negotiates 3.5).

The fix: a small standalone poller (`tinytuya`, Python) running on the Pi host, hitting the device directly over the local network every ~30 seconds, feeding Home Assistant via `command_line` + `template` sensors. No cloud dependency once set up — after a one-time key extraction via a free Tuya IoT developer project, the saved local key is all that's needed going forward.

<!-- TODO: add screenshot of the Home Assistant "Whirlpool" dashboard sidebar entry -->

### Entities

- **Water temperature** — from the TH Elite itself (Sonoff's own local integration), not the Tuya device
- **Water quality** — pH, ORP, TDS, salinity, conductivity, specific gravity, and a "Chlorine Factor" reading (kept only as an unvalidated curiosity — almost certainly a derived estimate, not an independent chlorine sensor) — all from the Tuya W218 via the custom poller above
- **Power/Auto Mode switches** — the TH Elite's relay, exposed directly

<!-- TODO: add screenshot of the Water Quality dashboard tab -->

### Automation: temperature control, nightly blackout, filter cycle

Three rules enforced entirely in Home Assistant (not the eWeLink app), because combining "temperature hysteresis," "never run at night," and "force a filter cycle every hour" reliably in the on-device Thermostat/Loop-Timer/Schedule modes isn't something these devices do cleanly — they're mutually-exclusive modes on the relay, not layerable rules:

- **09:00-18:00 only**: target 29°C, turns on below 28.5°C, off above 29.5°C — a periodic recheck every 5 minutes (not a crossing-triggered rule, which would miss the case where the temperature is already past a threshold when the automation starts or right after the nightly blackout)
- **Every hour on the hour, daytime only**: forces a 10-minute run regardless of temperature, for filtration/ozone circulation
- **18:00**: unconditional off, no exceptions — pump/ozonator never run 18:00-09:00

<!-- TODO: add screenshot of the automation config in Settings -> Automations -->

### Water Advice

A plain-language sensor checks current pH/ORP/TDS against Softub's own Water Treatment Guide targets (free chlorine 3-5 ppm via ORP as an automated stand-in since ppm isn't directly measurable here, pH 7.2-7.8) and tells you what to do — "pH high, add pH-minus," "ORP low, add a chlorine tab," etc. — plus a one-time push notification when something actually needs attention, not constant noise. Chlorine tabs are dosed reactively when this flags it, the same way pH-plus/minus are — there's no automatic chlorine dosing hardware in this build.

<!-- TODO: add screenshot of the Water Advice tile / a notification example -->
