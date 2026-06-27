# Changelog

## 2026-06-27 — Hardware Revision: Simplified Switching

### Changed
- Removed Shelly Dimmer 0/1-10V PM Gen3, Kemo M240, and Shelly 2.5 — TH Elite now switches pump, ozonator, and Isonic valve directly (all in parallel on the switched output)
- Replaced FQ-220 ozonator with FQT-124 corona-discharge ozonator (39.99 CHF)
- Isonic V1C06-AY1 magnetic valve (12VDC) reused from original Softub installation, powered via TH Elite's switched output through a 12V DC power supply
- Tuya W218 temperature probe relocated from Kemo housing to electronics cabinet
- Pump control changed from variable-speed (35–100%) to on/off — variable speed incompatible with this motor
- Updated wiring diagrams (SVG + HTML) to reflect simplified layout
- Added Table of Contents to COMPONENTS.md, GUIDE.md, APP-SETTINGS.md, and bill-of-materials.md
- BOM: Wago connectors, cable glands, and gel boxes moved to consumables
- BOM: Consumables section moved before Tools section

### Removed
- Shelly Dimmer 0/1-10V PM Gen3 (16.70 CHF)
- Kemo M240 power controller (41.90 CHF)
- Shelly 2.5 (12.00 CHF) — not needed when TH Elite switches everything directly
- LiYY 4x0.25mm² control cable (no longer needed without 0-10V signal)
- 0–10V control signal path from wiring diagrams

### Added
- 12V DC power supply for Isonic valve (~5.00 CHF estimated)
- Isonic valve section in COMPONENTS.md and GUIDE.md

## 2026-06-22 — Documentation Overhaul

### Added
- 28 curated build photos in `images/` (renamed, resized to 1600px for web)
- `docs/APP-SETTINGS.md` — Sonoff TH Elite, Shelly Dimmer, and Home Assistant configuration
- `bom/bill-of-materials.md` — full parts list with prices, source links, and computed totals (282.33 CHF)
- `diagrams/wiring-diagram.svg` — standalone SVG for GitHub rendering
- `CHANGELOG.md`
- Motivation section with failure story, official repair quote (CHF 1,100), and Balboa part numbers
- Disclaimer & Safety section with build rules, wire colour warning, and replication advice

### Changed
- Renamed Sonoff TH16A → TH Elite, Shelly Plus 0-10V → Dimmer 0/1-10V PM Gen3
- Replaced "spa" with "whirlpool", title to "Softub Whirlpool Smart Conversion"
- Clarified Venturi injector and check valve are original factory parts (reused)
- Updated wiring diagram: naming, removed backup section and RCD/project blocks
- BOM source names now link to product pages (Galaxus, AliExpress)
- Merged Water Care into Control & Smart Home in BOM
- Title case on all headings across all files

### Removed
- `docs/SAFETY.md` — merged into README
- `docs/TROUBLESHOOTING.md` — distributed to README and GUIDE
- `bom/bill-of-materials.xlsx` — replaced by Markdown
- `diagrams/conversion-overview.html`
- `LICENSE`
- Backup/spares, abandoned bypass relay design, repository structure, roadmap sections
