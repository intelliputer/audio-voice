# J2 Jumper Discrepancy Analysis

## Question

Why do Mattel's drawings and service documentation show a removable jumper block on the Intellivoice J2 stacking connector, while the repository's Rev B PCB photograph shows no jumper block or stacking connector?

## Finding

There is a real discrepancy between the documented J2/JB configuration and the photographed `3330-4229 PCB Rev B` board. The photograph is direct evidence that this particular board has an unpopulated J2 footprint; it cannot accept the illustrated removable jumper in its photographed state.

The present evidence does **not** prove whether this was a factory population change or a carefully removed connector. A factory omission is the leading hypothesis, but it must be verified on another intact unit.

## Direct hardware evidence

The repository's [component-side Rev B image](../images/intellivoice-board-top.jpg) shows:

- a two-row, 30-position footprint between U2/SPB-640 and U3/SP-0256;
- the `30` silkscreen marking at one end, matching J2's documented numbering;
- no 30-pin connector body fitted to that footprint; and
- no jumper block fitted.

The reverse-side image identifies the PCB as `MATTEL INC 1981`, `3330-4229`, `PCB REV B` and shows the corresponding soldered pad array. See [the full bottom-side photograph](../images/intellivoice-board-bottom.jpg) and the [annotated J2 area](../images/intellivoice-board-bottom-j2-annotated.png).

Because there is no connector body, a removable block could not be installed on this board as photographed. This corrects any interpretation based solely on the bottom-side image that the connector might merely be concealed on the other side.

## Documentation evidence

Two independent Mattel documents describe a different, J2-populated configuration.

### Product engineering specification

The [Intellivoice Product Engineering Specification](CCF10232011_00021.pdf) shows, in Figure 1, a port cover, J2, and an exploded-view "Jumper Block, JB1." Section 4.7 says that when J2 is not attached to another peripheral, jumper block `JB1` (part `3330-9609`) must be in place. It specifies two links:

- J2 pins 3--4;
- J2 pins 27--28.

The specification identifies J2 as a 30-contact stacking connector for future peripherals.

### Service manual

The [Intellivoice Service Manual](Intellivoice_Service_Manual_1979_Mattel_US.pdf) tells the technician to remove the port cover and check that the jumper is attached to the stacking connector. Its Figure 2 is titled "Inserting Jumper Block JB2." The parts list includes:

| Reference | Description | Mattel part number |
| --- | --- | --- |
| `JB2` | Jumper Block | `3330-9609` |
| `J2` | Connector, Stacking 30 pin | `0089-0021` |

`JB1` versus `JB2` is a documentation designator inconsistency. The shared jumper part number and function indicate one intended assembly, not two separate jumper blocks.

## What the evidence supports

| Claim | Status | Basis |
| --- | --- | --- |
| Mattel designed and documented a J2 connector with a removable jumper. | established | Both Mattel documents, including service instructions and part numbers. |
| The photographed Rev B board lacks the J2 connector and jumper. | established for that board | Component-side photograph and `PCB REV B` marking. |
| The 3--4 and 27--28 paths must be present somehow for the documented design. | established for the documented configuration | Product engineering specification. |
| Rev B implements those paths through traces or factory solder links. | plausible, unverified | The board has no documented jumper; continuity has not been measured. |
| Mattel omitted J2/JB at the factory on all Rev B boards. | unproven | Only one Rev B board has been inspected photographically. |
| Someone removed J2 from the photographed board. | possible, unproven | The photographs provide no chain of custody or pre-removal record. |

## Most likely interpretation

The leading explanation is a production-revision or population change: the board retained the J2 footprint but the photographed Rev B unit did not receive the connector or jumper. This fits an unused future-expansion feature being removed or de-populated while reusing the existing PCB artwork.

It should remain an inference, rather than a historical fact, until it is corroborated by another intact unit or by factory revision documentation. Connector removal by a former owner is less attractive because the footprint looks orderly, but careful rework cannot be excluded from photographs.

## Decisive survey for the spare Intellivoice

Do this with the module unpowered and before any rework:

1. Photograph the exterior label, serial number, port cover, and PCB revision markings.
2. Remove the port cover and photograph J2 square-on, plus both ends and the surrounding PCB.
3. Record whether J2 is populated and whether a jumper block is installed.
4. If J2 is populated, document the jumper's orientation before removing it.
5. Measure continuity for J2 3--4 and J2 27--28.
6. If J2 is unpopulated, determine whether each pair is joined by a PCB trace, a solder bridge, or neither.
7. Photograph the solder side; do not desolder anything during the initial survey.

### Interpretation of the results

| Spare-unit result | Meaning |
| --- | --- |
| Populated J2 with jumper present | Confirms a genuine production configuration difference from the photographed Rev B board. |
| Unpopulated J2, same `3330-4229 Rev B`, both pairs continuous | Strong evidence of a factory Rev B bypass/population change. |
| Unpopulated J2, same revision, either pair open | Indicates the specification/service documentation is not electrically applicable to that revision; investigate before powering or modifying. |
| Connector/removal evidence or a different PCB revision | Record it as a separate variant; do not generalize from one board. |

## Practical consequence

Do not design a plug-in J2 adapter around the Rev B photograph. A connector may not be installed, and the jumper's electrical pass-throughs may be implemented differently. Treat J2 population, geometry, and continuity as revision-specific observations until the spare-unit survey is complete.

## Sources

- [Intellivoice Product Engineering Specification](CCF10232011_00021.pdf), Figure 1 and section 4.7.
- [Intellivoice Service Manual](Intellivoice_Service_Manual_1979_Mattel_US.pdf), Figure 2, troubleshooting checklist, and parts list.
- [Rev B top-side PCB photograph](../images/intellivoice-board-top.jpg).
- [Rev B bottom-side PCB photograph](../images/intellivoice-board-bottom.jpg).
- [The Liberator Rev B reference photos](https://www.the-liberator.net/site-files/retro-games/hardware/Intellivision-Intellivoice/intellivision-intellivoice-voice-synthesis-module-rev-b.htm).
