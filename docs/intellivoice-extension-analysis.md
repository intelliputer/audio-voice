# Intellivoice Extension Analysis

## Finding

The Intellivoice Voice Synthesis Module (product 3330) was explicitly designed to serve as a base for future Intellivision peripherals. Its unused, top-mounted **J2 stacking connector**, beneath the removable cover, is the intended expansion interface.

The product engineering specification says that the module included additional hardware for controlled communication with future peripherals and that those peripherals would attach to the top-mounted connector. This makes J2 a genuine system-expansion interface, rather than an unconnected test or service header.

## What J2 exposes

The module's buffer/interface chip (U2) connects the Master/Keyboard Component to both the speech synthesizer and J2. The specification documents the following facilities for an attached peripheral:

- An 8-bit bidirectional data bus.
- Master Component bus-control signals: `BC1`, `BC2`, and `BDIR`.
- Three Intellivoice-generated peripheral address-select signals: `AD0`, `AD1`, and `AD2`.
- `RMHC`, which controls the Keyboard Component's bus-buffer direction.
- The Master Component clock (`MCLK`), routed through J2.
- A power-support path intended to augment the Master/Keyboard Component's supply.

The data bus is available at J2 when the Master Component writes to the Intellivoice address range `0x0800`–`0x0BFF`. The address-select lines provide one-of-eight peripheral selection. Together, these facilities make a memory-mapped, bus-controlled peripheral viable without modifying the console.

## Historical intended use

The historical record identifies **wireless hand controllers** as the only peripheral known to have been planned for this connector. The concept disappeared when the proposed Intellivision III, which was also to include wireless controllers, was cancelled. No retail peripheral is known to have used the J2 connector.

That makes a wireless-controller receiver the most historically faithful modern extension. A compact board could receive modern wireless-controller input and present it to the Intellivision through the documented peripheral interface.

## Other viable modern extensions

- **Controller and input hub:** wired or wireless controller adapters, additional inputs, and controller diagnostics.
- **Hardware diagnostic interface:** bus monitoring, ROM/cartridge checks, speech-chip tests, and voltage measurements for restoration work.
- **General-purpose I/O peripheral:** a microcontroller-backed GPIO, serial, or storage interface, accessed through the documented memory-mapped bus.
- **Speech-development tool:** hardware to observe or generate data around the speech buffer's serial/FIFO workflow. This is an experimental aid, not a replacement for the original speech synthesizer.

## Integration constraints

J2 is not plug-and-play in the modern sense. When no peripheral is connected, the factory jumper block (JB1) must be present: it shorts pins 3 and 4, and pins 27 and 28. Without the jumper—or an extension that correctly reproduces its pass-through functions—the Intellivoice and cartridge will not receive required power or clock routing.

The specification also documents an optional power-injection path on J2 pin 6. It was designed to support the Intellivoice and cartridge load and boost the Master/Keyboard Component supply by approximately 270 mA through an 8.2-ohm, 2-watt resistor. Any modern board should leave this path unused unless its power design, voltage limits, polarity, protection, heat dissipation, and console condition have been fully validated.

The first prototype should therefore be a passive or low-power interface that preserves the jumper's signal paths, avoids injecting power, and uses conservative, console-compatible logic levels. Pinout, directionality, timing, and bus ownership should be verified against the original schematic and a real unit before connecting active electronics.

## Recommended direction

Build a **J2 wireless-controller and diagnostics hub** in two stages:

1. Create a safe pass-through adapter that preserves JB1's two required links and breaks out J2 for measurement.
2. Add a low-power microcontroller peripheral, initially limited to passive bus observation and controller diagnostics; only then enable bus responses and wireless-controller support.

This approach honors the original product's intended expansion role while minimizing risk to scarce vintage hardware.

## Sources

- [Intellivoice Product Engineering Specification (local PDF)](CCF10232011_00021.pdf), especially sections 3.1–3.2, 4.5, and 4.7.
- [Technical overview and hardware history — Blue Sky Rangers](https://history.blueskyrangers.com/hardware/intellivoice.html).
- [Specification transcription: stacking-connector bus details](https://papaintellivision.com/pdfs/CCF10232011_00021.pdf).
