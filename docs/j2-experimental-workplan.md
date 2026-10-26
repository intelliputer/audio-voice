# J2 Experimental Workplan

## Objective

Use a spare Intellivoice Model 3330 to establish, with reproducible evidence, whether its J2 stacking-connector provision can support a modern peripheral or a limited Master/Keyboard Component compatibility bridge. The work must produce engineering records useful to others and must not rely on assumptions from photographs or OCR alone.

This is a characterization project first. Building a functional peripheral is a later decision, contingent on the evidence gathered here.

## Scope and non-goals

### In scope

- Identify the spare unit's PCB revision and J2 implementation.
- Determine whether the J2 holes are factory-filled, connector-removal remnants, or an alternative production arrangement.
- Build a verified pinout and continuity map.
- Measure J2 electrical behavior with the module operating normally.
- Establish a safe path to a passive breakout and, only if justified, an active test peripheral.
- Document the degree to which J2 can communicate with the Master/Keyboard Component bus.

### Out of scope for the first campaign

- Injecting power at J2 pin 6.
- Directly attaching a microcontroller or USB adapter to J2.
- Modifying the known-good/reference Intellivoice.
- Claiming to reproduce the full Keyboard Component before its separate hardware/software behavior is characterized.

## Evidence standard

Every result should have an evidence level:

| Level | Meaning |
| --- | --- |
| `D` | Documented in an original manual/specification. |
| `O` | Directly observed and photographed/measured on the spare unit. |
| `V` | Verified by a repeatable test with recorded setup and result. |
| `H` | Hypothesis only; not suitable as a design input. |

Do not convert `D` or `H` into `V` without an actual measurement. Record disagreements between documentation and the unit rather than reconciling them by guesswork.

## Equipment and preparation

Minimum equipment:

- A known-good Intellivision Master Component, power supply, RF/composite display path, and a known-good Intellivoice-compatible cartridge.
- The spare Intellivoice and a separate known-good/reference Intellivoice, if available.
- Digital multimeter with continuity and DC-voltage modes.
- Fine probes, insulated grabbers, and ESD mat/wrist strap.
- High-resolution camera, tripod/phone mount, metric ruler, and calipers.
- Bench supply with current limit **only for separate, unconnected experiments**; do not use it to feed J2 during the initial work.

Strongly recommended:

- Oscilloscope with ×10 probes.
- Logic analyzer with high-impedance inputs and a sampling rate comfortably above the observed bus/clock rate.
- Soldering station, desoldering tool, microscope, flux, and isopropyl alcohol—held in reserve until the non-destructive phases are complete.
- A custom test cartridge or development cartridge capable of executing controlled bus writes.

Create a dated experiment folder for every session. Store original, uncropped photographs alongside any annotated copies.

## Phase 0 — baseline and chain of custody

**Goal:** establish that the spare is a working, identifiable artifact before any intervention.

1. Photograph all exterior surfaces, labels, serial/revision markings, port cover, and screws before opening the case.
2. Run the spare in a stock Master Component with a known-good voice-compatible game.
3. Record startup behavior, speech output, independent voice-volume operation, and any faults.
4. If possible, perform the same short test with a reference Intellivoice under the same console/cartridge/power conditions.
5. Log the exact console, cartridge, power supply, display/audio setup, room date, and operator.

**Artifacts:** `00-baseline.md`, exterior photo set, and a short baseline video/audio recording.

**Gate:** stop if the spare is not stable in normal use. Repair baseline hardware behavior before interpreting J2 results.

## Phase 1 — PCB and J2 mechanical survey

**Goal:** determine exactly what is physically present at J2.

1. Open the spare without desoldering or removing components.
2. Photograph both PCB sides square-on at maximum resolution, plus macro photos of J2 from four directions.
3. Photograph the port cover and any jumper/block separately, with a scale.
4. Record PCB revision, date codes, silkscreen identifiers, and every J2/JB marking.
5. Compare the unit with the Rev B reference photos and the service-manual parts list; record differences explicitly.
6. Measure the J2 footprint:
   - total positions and rows;
   - centre pitch across at least ten positions;
   - row spacing;
   - plated-hole diameter;
   - pad diameter/shape;
   - available component-side and case clearance;
   - position relative to the port cover and PCB edges.
7. Inspect solder fill under magnification. Record whether it appears wave-soldered, hand-soldered, intentionally bridged, damaged, or consistent with a removed component.

**Artifacts:** `01-j2-mechanical-survey.md`, raw and annotated photos, a dimensioned drawing, and a revision comparison table.

**Gate:** do not select a connector, desolder holes, or order an adapter until this record establishes the footprint geometry and the most likely solder-fill condition.

## Phase 2 — unpowered continuity map

**Goal:** validate the documented J2 signal map against the actual unit.

1. Keep the module disconnected from all power.
2. Locate the J2 positions from the specification's Figure 1 and verify orientation against the PCB.
3. Check continuity from each J2 position to accessible U2/U3 pins, J1, J3, test points, ground, and power rails.
4. Specifically verify the documented jumper paths: J2 3–4 and J2 27–28. Record whether they are connected by a removable jumper, a solder bridge, PCB traces, or not connected.
5. Build a pin table with actual measured destinations—not inferred signal names—and then add the corresponding documented signal name if it matches.
6. Repeat ambiguous measurements with a second probe point or a second meter.

Suggested pin-table fields:

| J2 position | Physical location/orientation | Resistance to GND | Continuity destination | Documented signal | Evidence | Notes |
| --- | --- | --- | --- | --- | --- | --- |

**Artifacts:** `02-j2-continuity-map.csv`, annotated PCB images, and a short method note.

**Gate:** the pinout must be complete enough to distinguish ground, supply-related paths, control signals, and the eight data lines before the unit is powered for J2 observation.

## Phase 3 — powered passive observation

**Goal:** observe J2 without changing its electrical behavior.

1. Restore the module to a known-good, stock configuration, including all required jumper paths.
2. Confirm the Phase 0 baseline again after reassembly.
3. Measure DC voltage at each suspected supply-related J2 position relative to ground; do not connect external power.
4. Using ×10/high-impedance probes, observe `MCLK`, `BC1`, `BC2`, `BDIR`, `AD0`–`AD2`, and selected data lines during startup and normal voice-game play.
5. Capture waveforms with complete metadata: probe type, ground point, bandwidth limit, timebase, trigger, console/cartridge, and image filename.
6. Compare behavior with a non-voice cartridge, a voice-compatible cartridge, and—if available—a Keyboard Component/ECS configuration.
7. Watch for changes in signal direction and activity rather than attempting to decode all traffic immediately.

**Artifacts:** `03-passive-observation.md`, scope captures, logic-analyzer traces, and a voltage table.

**Gate:** no active attachment until voltage ranges, idle behavior, active timing, and bus ownership are measured and reviewed.

## Phase 4 — custom software characterization

**Goal:** deliberately exercise the documented address/decode path.

The specification says U2 exposes J2 data during writes in the `0x0800`–`0x0BFF` range and provides `AD0`–`AD2` selection states. Confirm that behavior with purpose-built code rather than relying solely on game traffic.

1. Create a minimal test program that writes repeatable patterns (`0x00`, `0xFF`, walking 1s, alternating bits, incrementing bytes) across the relevant addresses.
2. Begin with single writes separated by long delays, then controlled bursts.
3. Capture all data lines, control signals, address-select lines, and `MCLK` simultaneously if equipment permits.
4. Make a timing diagram for each verified cycle: address select, control state, data valid period, and data direction.
5. Test reads only after writes and direction behavior are understood; do not permit an external device to drive data during early read tests.

**Artifacts:** source/binary for the test cartridge, `04-address-decode-results.md`, raw captures, and verified timing diagrams.

**Gate:** proceed only when the bus cycle can be reproduced from the test program and the results agree with, or precisely document divergence from, the original specification.

## Phase 5 — connector decision and passive breakout

**Goal:** decide whether a connector should be installed and validate a removable breakout.

Decision options:

| Condition | Next action |
| --- | --- |
| J2 is populated and a compatible connector can be identified | Build a removable mating breakout. |
| J2 is unpopulated but clean | Create a sacrificial prototype only after documenting the modification and verifying footprint dimensions. |
| J2 holes are factory-filled or mechanically uncertain | Do not desolder yet; use further inspection, donor-board comparison, and connector research. |
| Baseline operation is unstable | Stop; do not modify the unit. |

If connector installation is approved:

1. Preserve raw photographs and a complete continuity map before any desoldering.
2. Practice solder removal and connector fit on a non-valuable test board with comparable holes.
3. Clear one position at a time with temperature-controlled tooling; inspect pads and vias under magnification.
4. Install the smallest, lowest-risk header compatible with the measured footprint and enclosure clearance.
5. Build a passive breakout that preserves the two documented jumper paths, contains no active electronics, and exposes labelled test pads.
6. Re-run Phase 0 and Phase 3 tests after every hardware change.

**Artifacts:** connector-selection rationale, bill of materials, installation photo log, breakout schematic/PCB files, continuity report, and regression test results.

**Gate:** do not attach an active board if the passive breakout changes voice operation, console operation, voltage levels, or observed timing.

## Phase 6 — active, read-only peripheral

**Goal:** attach a high-impedance monitor that cannot contend for the data bus.

1. Use voltage-appropriate, high-impedance buffers. Do not assume modern microcontroller I/O is 5 V tolerant.
2. Add no pull-ups/pull-downs until their effect has been modeled and measured.
3. Record bus cycles to removable storage or an isolated serial link.
4. Compare passive-probe captures with monitor captures to establish that the monitor is electrically transparent.
5. Decode trace data into a documented protocol hypothesis.

**Artifacts:** monitor schematic/firmware, electrical design review, before/after waveform comparison, trace decoder, and protocol notes marked as `H` until independently verified.

**Gate:** only consider bus driving after transparent monitoring is proven and an explicit read-cycle protocol has been measured.

## Phase 7 — narrow active-function proof of concept

**Goal:** implement one deliberately small response, not a full Keyboard Component replacement.

Candidate first functions:

- a read-only identification/status register;
- a latched LED or diagnostic register;
- controller-input capture exposed to custom software;
- a simple Keyboard Component-facing compatibility experiment.

Requirements:

- Address decode must select the device unambiguously.
- The device must drive `D0`–`D7` only during an experimentally verified read window.
- All data outputs must return to high impedance outside that window.
- The test program must detect and report bus contention/failure safely.
- Current must be measured before and during operation.

**Artifacts:** protocol specification, firmware, hardware design files, timing budget, test results, and a known-limitations section.

## Power investigation: separate track

The specification describes J2 pin 6 as an unregulated supply-support input, routed through R26 (8.2 ohm, 2 W) to provide an approximately 270 mA boost to the Master/Keyboard Component supply. This is a historical provision, not a ready-made accessory power input.

Do not test it until all of the following are complete:

- the exact J2 pin-6 route is confirmed by continuity map;
- console and Intellivoice current draw are characterized under normal operation;
- the input-voltage range and protection behavior are established from primary documentation or controlled bench work;
- a current-limited, fused supply setup and emergency disconnect are prepared;
- an explicit test plan has been reviewed.

The first power experiment, if authorized later, should use a dummy-load or isolated board-level model—not a valuable live console.

## Keyboard-compatibility research track

J2's `RMHC` signal is documented as controlling the Keyboard Component's bus-buffer direction, which makes keyboard-oriented experiments reasonable. It does not, by itself, recreate Keyboard Component behavior.

Before attempting emulation:

1. Acquire and archive primary Keyboard Component/ECS schematics, service documentation, and bus specifications.
2. Build a separate comparison matrix: signal names, voltage levels, direction, timing, pull resistors, address decode, reset behavior, and power domains.
3. Capture normal bus behavior with the genuine Keyboard Component/ECS where available.
4. Identify one narrowly scoped function to emulate; do not begin with an entire keyboard, BASIC environment, storage subsystem, or power system.
5. State precisely which external software must support the experiment.

## Documentation deliverables

At campaign completion, publish or retain:

- revision-specific mechanical drawing of J2;
- raw, unmodified photo archive;
- verified J2 pinout and continuity table;
- voltage and timing measurements with test conditions;
- annotated waveform/logic traces;
- custom test-program source and reproducible build instructions;
- connector and breakout design files, if created;
- a change log separating original hardware state from all modifications;
- a results summary listing verified findings, unresolved questions, and next-step recommendations.

## First-session checklist

- [ ] Verify stock voice-game operation.
- [ ] Photograph exterior, port cover, jumper, and both PCB sides.
- [ ] Record PCB revision and all part markings.
- [ ] Measure J2 footprint geometry and inspect solder fill.
- [ ] Confirm the documented jumper paths with the unit unpowered.
- [ ] Create the first version of the J2 pinout table.
- [ ] Stop before desoldering, connector selection, power injection, or active electronics.

## Reference material

- [J2 reverse-engineering guide](j2-stacking-connector-reverse-engineering.md)
- [Extension analysis](intellivoice-extension-analysis.md)
- [Peripheral concepts](intellivoice-peripheral-concepts.md)
- [Intellivoice Product Engineering Specification](CCF10232011_00021.pdf)
- [Intellivoice Service Manual](Intellivoice_Service_Manual_1979_Mattel_US.pdf)
