# Changelog

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
