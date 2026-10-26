## Intellivision Audio Voice

This repository is a compact hardware-reference archive for Mattel's Intellivoice Voice Synthesis Module (product 3330), the add-on that gives compatible Intellivision games synthesized speech. It is intended for preservation, repair, reverse-engineering, and historical research rather than as a software project.

The product engineering specification describes how the module sits between the Intellivision Master/Keyboard Component and a cartridge: a speech synthesizer and buffer/interface chip receive speech data, while the onboard filter/amplifier converts the synthesizer's pulse-width-modulated output into the console's audio path. The unit passes through non-voice cartridges, adds speech when a cartridge supports it, and includes an independent voice-volume control.

### Contents

- [Product engineering specification](docs/CCF10232011_00021.pdf) — a 52-page Intellivoice reference covering product operation, functional breakdown, assembly information, signals, and test/acceptance details. The document refers to the speech synthesizer, buffer/interface, audio section, connector interfaces, and related schematic/PCB documentation.
- [Service manual scan](docs/Intellivoice_Service_Manual_1979_Mattel_US.pdf) — original service documentation.
- [Extension analysis](docs/intellivoice-extension-analysis.md) — research on the unused J2 stacking connector, its planned role, and safe modern-extension directions.
- [Peripheral concepts](docs/intellivoice-peripheral-concepts.md) — possible J2-connected devices for input, diagnostics, storage, communications, and audio experimentation.
- [J2 reverse-engineering guide](docs/j2-stacking-connector-reverse-engineering.md) — a non-destructive process for identifying J2's mating interface and validating a breakout adapter.
- [J2 experimental workplan](docs/j2-experimental-workplan.md) — evidence-gated characterization, breakout, bus-observation, and compatibility-bridge research plan for a spare module.
- [Intellivision schematic](images/Intellivision-Schematic.png) — a high-resolution hardware schematic image for context and cross-reference.
- [Motherboard photograph](images/motherboard.jpg) — a board-level visual reference.

### Scope

There is no emulator, firmware source, or build system in this repository. The files are primary-reference material and images; consult the PDFs at full size when working from pinouts, component values, or service procedures.

### License

The repository is distributed under the [GNU General Public License v3.0](LICENSE). Original historical documents and images may carry their own rights; verify their status before redistributing or using them beyond research and preservation.
