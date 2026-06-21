# Changelog

## 2026-06-21 — Documentation Overhaul

### Added
- `images/` folder with 20 curated build photos (renamed from raw camera files), covering disassembly, original components, new smart components, and reassembly
- Photo references throughout all documentation files
- Expanded disclaimer and safety section in `README.md` covering: no professional qualification, no liability, no guarantee of correctness, local regulations, insurance/warranty implications, and build safety rules
- Table of contents in `README.md`
- Motivation section with full failure story, repair quote, and Balboa part numbers
- `docs/APP-SETTINGS.md` for software-side configuration (Sonoff TH Elite, Shelly, Home Assistant)
- `bom/bill-of-materials.md` (converted from Excel, with resolved formulas and computed totals)

### Changed
- Renamed title from "Softub Smart Conversion" to "Softub Whirlpool Smart Conversion"
- Replaced "spa" with "whirlpool" across all docs
- Renamed "Sonoff TH16A" to "Sonoff TH Elite" across all docs
- Renamed "Shelly Plus 0-10V Dimmer" to "Shelly Dimmer 0/1-10V PM Gen3" across all docs
- Rewrote disclaimer section from a short paragraph to a structured list with safety rules
- Clarified Venturi injector and check valve are original factory parts (reused, not new)
- Updated wiring diagram to match all documentation changes

### Removed
- `docs/SAFETY.md` — merged into README Disclaimer & Safety section
- `docs/TROUBLESHOOTING.md` — content distributed to README (motivation, safety) and GUIDE (removed parts)
- `diagrams/conversion-overview.html`
- `LICENSE` file
- `bom/bill-of-materials.xlsx`
- Repository structure, roadmap, and license sections from README
- Backup/spares section from BOM and COMPONENTS
