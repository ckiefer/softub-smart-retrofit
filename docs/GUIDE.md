# Build Guide

## Table of Contents

- [1. Remove the Original Electronics](#1-remove-the-original-electronics)
- [2. Mount the Terminal Block](#2-mount-the-terminal-block)
- [3. Wire the Capacitor](#3-wire-the-capacitor)
- [4. Wire the Sonoff TH Elite](#4-wire-the-sonoff-th-elite)
- [5. Wire the Ozonator and Isonic Valve](#5-wire-the-ozonator-and-isonic-valve)
- [6. Wire the Tuya W218 Water Analyzer](#6-wire-the-tuya-w218-water-analyzer)
- [7. Final Assembly](#7-final-assembly)
- [8. Test Before Refilling](#8-test-before-refilling)

A walkthrough of the conversion in the order it was actually done. Read the [Disclaimer & Safety](../README.md#disclaimer--safety) section in the README first.

## 1. Remove the Original Electronics

Unplug the whirlpool. Wait at least 5 minutes and discharge any capacitors before touching anything.

![Softub rating plate — 230V ~50Hz, 6.0A, 1400W, IPX5 Class 1](../images/07-softub-rating-plate.jpg)

![Original Softub topside control panel](../images/08-softub-topside-panel.jpg)

![Original control board and motor assembly, top view](../images/02-original-control-board-overview.jpg)

Photograph and label every wire before disconnecting it — the original board's labelling (relay, transformer, valve) is not obvious once it's in pieces. Remove:

- The main control PCB (Softub Inc., marked "C-2013" on the unit used here)
- The Zettler relay (rated 220V coil, 30A/277VAC contacts — this was the switching element for the pump)

![Zettler AZ2700-2A-220A relay — removed](../images/13-zettler-relay.jpg)
- The Isonic solenoid valve (V1C06-AY1, 12VDC, normally-closed) — **set aside for reuse** in the new ozone circuit, where it will be switched by the TH Elite via a 12V DC power supply

![Isonic V1C06-AY1 solenoid valve — removed from original board, reused in new circuit](../images/12-isonic-solenoid-valve.jpg)

- The Aquatemp transformer (230V primary, 12V secondary — only powered the control logic, not the motor)
- The original AquaSunOzone XL-30 ozonator (12V DC — replaced by a new 220V passive unit that doesn't need the transformer)

![Original AquaSunOzone XL-30 ozonator — old unit, was installed in the motor housing](../images/03-original-ozonator-label.jpg)

![Original capacitor with spade terminals — removed](../images/06-original-capacitor-terminals.jpg)

![Motor wires — US color convention (green=earth, black, white/yellow)](../images/10-motor-wires-us-colors.jpg)

![Motor internal terminals — corroded, needed repair](../images/11-motor-internal-terminals.jpg)

![Balboa pump/motor assembly with heater coil and foam insulation](../images/15-motor-assembly-removed.jpg)

None of these are reused, except the Isonic solenoid valve which is reinstalled in the new ozone circuit. The original pool LED light was also not re-installed — it would have required additional components (driver, controller) for little practical benefit.

![Original PCB internals — Zettler relay, transformers, wiring](../images/04-original-pcb-internals.jpg)

![Original PCB still mounted in aluminum bracket](../images/21-original-pcb-in-housing.jpg)

![Original PCB removed, laid flat — Softub C-2013, serial 9037622](../images/22-original-pcb-removed-flat.jpg)

![Original Isonic ozonator mounted in housing corner with yellow tubing](../images/24-original-isonic-in-situ.jpg)

![Unscrewing copper mounting bracket that held the original control board](../images/26-copper-mounting-bracket.jpg)

![Unscrewing motor housing cover — rusted screws, temperature probe visible](../images/27-unscrewing-motor-housing.jpg)

## 2. Mount the Terminal Block

Install on the left side of the motor housing, positioned so all downstream components can reach it with reasonably short runs. This is the single point all new wiring radiates from — getting its position right first makes everything after it easier.

## 3. Wire the Capacitor

![Original CSC capacitor label — 20uF, 370VAC, dated 2014](../images/05-original-capacitor-label.jpg)

- Mount the new capacitor on the left side of the housing, next to the terminal block.
- Confirm capacitance and voltage rating against the motor's original capacitor or nameplate spec (this build used 20µF / 450VAC as a replacement).
- Crimp insulated 6.3mm spade connectors onto the motor and capacitor leads — see [COMPONENTS.md](COMPONENTS.md) for crimping tool/die notes.
- The capacitor has two electrically-identical spade terminal pairs per side; use one per side and insulate the unused spare with heat shrink, since it sits exposed inside a housing that does see condensation.

## 4. Wire the Sonoff TH Elite

- Terminal block L/N → TH Elite IN-L/IN-N
- TH Elite OUT-L/OUT-N → pump motor live, ozonator, and 12V DC power supply for Isonic valve (all in parallel — see wiring diagram)
- Pump motor neutral → terminal block neutral
- DS18B20 → TH Elite's dedicated sensor input
- Mount the DS18B20 probe in the same position where the original temperature sensor sat, sealed in place with silicone to keep it waterproof and in direct contact with the water

## 5. Wire the Ozonator and Isonic Valve

![Original AquaSunOzone XL-30 for reference — replaced by the FQT-124](../images/03-original-ozonator-label.jpg)

![Isonic V1C06-AY1 solenoid valve — reused from original, switched via TH Elite](../images/12-isonic-solenoid-valve.jpg)

![Original Isonic valve in situ — mounted in housing corner](../images/24-original-isonic-in-situ.jpg)

<!-- TODO: add photo of new FQT-124 ozonator installed -->

- Mount the FQT-124 ozonator on the right side of the housing.
- Wire the ozonator to the TH Elite's switched output (230V), in parallel with the pump.
- Install the Isonic magnetic valve between the ozonator's air output and the existing ozone tubing. The valve is powered from the same switched output via a 12V DC power supply.
- When the TH Elite relay closes, the pump, ozonator, and Isonic valve all power on together. The Venturi vacuum from the running pump pulls ozone through the open valve and into the water.
- The Venturi injector and check valve in the water line are original Softub parts and stay in place.

- This ozonator has no internal air pump — it cannot push ozone into still water. Since everything is switched together by the TH Elite, ozone always flows when the system is active.

## 6. Wire the Tuya W218 Water Analyzer

- Mount the Tuya W218 on the right side of the housing.
- The W218 runs on 24V via its own power adapter — use a short extension cable to reach the nearest mains outlet from the terminal block.
- Tap power directly from the terminal block (own L/N leads), independent of the TH Elite switched circuit, so it stays on even when the pump cycle is off.
- pH, ORP, and TDS probes go into the pool water.
- The temperature probe in this build does **not** go into the water — it's mounted inside the electronics cabinet to monitor enclosure temperature.

## 7. Final Assembly

![TH Elite wired to pump, ozonator, and Isonic valve](../images/18-new-th-elite-wired.jpg)

![New components packed into motor housing with foam padding](../images/19-housing-reassembly.jpg)

![Reassembly top view — components, cable glands, ozone tubing](../images/23-reassembly-top-view.jpg)

- Route all 230V wiring and all low-voltage signal/sensor wiring with as much physical separation as the enclosure allows.
- Every junction inside the housing goes through a sealed gel-box connector (Wago) — condensation inside this enclosure is routine, not an edge case.
- Pack foam/padding material where needed to keep components from rattling against the housing or each other — offcuts work fine, this doesn't need to be pretty.

## 8. Test Before Refilling

1. With the whirlpool still empty of water, plug in and verify the TH Elite powers on and its display/app shows correctly.
2. Briefly run the pump dry (a few seconds only) to confirm rotation and that nothing smells hot or sounds wrong, then stop before any heat buildup.
