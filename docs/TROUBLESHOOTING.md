# Troubleshooting

Real problems encountered during this build, and how they were actually diagnosed — kept as-is rather than cleaned up, since the diagnostic process is often more useful than the fix itself.

## Pump didn't respond at all (Shelly showed a percentage, motor stayed silent)

**Symptom:** Shelly app showed 90% on the slider. Multimeter measured **0V** at the Shelly's own +/– signal output terminals (measured directly there, with the Kemo disconnected to isolate the test).

**Diagnosis path:**
1. First checked for 230V at the Kemo's mains input — present, confirmed by its green LED.
2. Suspected a wiring fault between Shelly and Kemo, but a direct measurement at the Shelly's own output terminals (bypassing the Kemo entirely) showed nothing — so the fault was upstream of the Kemo, at the Shelly itself.
3. Checked the Shelly's input/output configuration in the app: it was set to **"Single-button dimming"** mode, which expects a physical push-button wired to S1/S2 to function and apparently does not reliably drive the analog output without one.
4. Changed the mode to **"Detached switch"** (decouples the dimmer output from any button-input state) — still no voltage.
5. Checked "Action on power-on": it was set to **"restores last mode"**. Changed to **"turns on when powered"**.
6. Still nothing. Went back to the device's main dashboard screen and found a separate physical-looking power toggle next to the percentage slider — **this was off**. The percentage value alone is just a stored setpoint; the device also needs its own "on" state for the analog output to actually be driven. Turning this on resolved the issue.

**Takeaway:** on this device, three independent things all need to be correct simultaneously: (1) the input/output decoupling mode, (2) the power-on behaviour, and (3) the actual on/off toggle being in the "on" position. A percentage value showing in the app does not guarantee voltage is present on the output.

## Wire colour conventions were inconsistent across components

Two different imported components in this build used **opposite** colour conventions from each other:

- The ozonator: white = live, black = neutral
- The Balboa motor: black = live, white = neutral

Both are internally consistent with *some* regional convention (this generally tracks with US/Asian manufacturing vs. EU manufacturing, though it's not universal), but neither matches the other, and neither matches EU convention (brown = live, blue = neutral) used for the mains-side wiring in this build.

**Resolution:** every component's wiring was verified independently with a multimeter rather than assumed from colour, using simple resistance/continuity checks:
- To find which lead is earth on a 3-wire unknown cable with no metal chassis to reference against (e.g. a sealed plastic device), measure resistance between all three pairs of leads. Two leads will show a resistance value (these are the winding/load — live and neutral, in either order). One lead will show open circuit to both others (this is earth, isolated internally for protective purposes only).
- For polarity within a single-phase capacitor-start motor (which lead is live vs. neutral), it generally doesn't matter electrically — swapping the two simply reverses motor rotation direction with no risk of damage, so it can be determined empirically by testing actual pump flow direction rather than by trusting any colour code.

**Practical consequence:** swapping live/neutral on this particular motor was confirmed safe to test empirically (reversed rotation, fixed by swapping the two leads), but this should not be assumed true for every motor — check for a centrifugal start switch or any polarity-sensitive electronics before relying on this for a different unit.

## No ozone bubbles despite the ozonator clearly running

**Symptom:** ozonator powered on (confirmed — audible/visible activity from the unit), air tubing connected, check valve installed, but no bubbles visible at the water outlet.

**Diagnosis path:**
1. Checked the check valve orientation — correct.
2. Tested with the diffuser stone removed entirely, holding the bare tubing end underwater — still nothing.
3. Realized the unit is a **passive** ozonator: it has no internal air pump and produces ozone via corona discharge only. It relies entirely on the Venturi effect from water flowing past a constriction in the plumbing to create the vacuum that actually draws ozone gas into the water stream.
4. With the pump running (water actually flowing through the Venturi fitting), bubbles appeared immediately.

**Takeaway:** a passive ozonator producing visible/audible internal activity is not the same as it successfully dosing the water — it requires water flow through a Venturi injector to do anything. Always test with the pump running, not just the ozonator alone.

## Capacitor measurement gave a fluctuating, nonsensical reading

**Symptom:** measuring the new CBB60 capacitor's capacitance, the multimeter display climbed past 40 and then dropped to 0, repeating.

**Diagnosis:** the meter was still set to its resistance/continuity range (showing MΩ), not its capacitance range. Capacitance measurement was a secondary function on this meter, reached via the SELECT button while on a specific dial position, not its own dedicated dial setting. Once switched to the correct mode, the reading stabilized at the expected value.

**Takeaway:** worth double-checking which exact function a multimeter is in before trusting an odd reading — especially on meters where capacitance, frequency, or other secondary functions share a dial position with resistance and are toggled by a mode button rather than the main dial.

## Abandoned design: hardware bypass relay for the Kemo

Not a bug, but a design path worth recording since it was fully worked out before being dropped.

**Goal:** let the pump run at true 100% power without the Kemo M240 carrying the full load continuously (to keep its internal electronics cooler during extended high-power "jet mode" use).

**Design:** a changeover relay (COM/NC/NO) wired so that with the relay de-energized, mains power passed through to the Kemo as normal (NC path); energizing the relay would simultaneously cut power to the Kemo and route mains directly to the pump motor instead (NO path) — making it physically impossible for both paths to be live at once, by construction rather than by software interlock.

**Why it was dropped:** the Sonoff Mini R2 considered for this doesn't expose its relay as a usable changeover pair for this purpose — it would have needed an additional external relay (e.g. a Finder 40-series with a real COM/NC/NO pinout) just to act as the actual switching element, with the Sonoff only driving that relay's coil. That's a reasonable design, but added a second relay, additional wiring, and additional points of failure for a feature (keeping the Kemo cooler during occasional full-power use) that turned out not to be worth the complexity. The final build simply extended the Shelly's dimmer range to 35–100% and accepts that the Kemo runs warmer during sustained jet-mode use — checked by hand occasionally rather than engineered around.
