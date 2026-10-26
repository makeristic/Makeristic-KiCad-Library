# Makeristic KiCad component library

A collection of KiCad schematic symbols, PCB footprints, and 3D models discovered or customised for Makeristic electronics projects. It includes RF modules, an antenna, an STM32WB55 matching filter, switches, a MOSFET, and microcontroller symbols.

## Contents

- [Makeristic-KiCad-Library.lib](Makeristic-KiCad-Library.lib): 10 legacy schematic symbols, with descriptions and datasheet links in [Makeristic-KiCad-Library.dcm](Makeristic-KiCad-Library.dcm).
- [Makeristic-KiCad-Library.pretty/](Makeristic-KiCad-Library.pretty/): 12 footprints, including E73-2G4M08S1C, FR05-S1-N-0-102, MLPF-WB55-01E3, NRF9160, PCM12SMTR, SKRPADE010, STL6P3LLH6, three STM32F042 package variants, a mounting hole, and a STEMMA QT logo.
- [Makeristic-KiCad-Library.3dshapes/](Makeristic-KiCad-Library.3dshapes/): nine STEP/STP/VRML models, including STM32WB55, NRF9160, E73, USB-C, and JST connectors. Not every symbol or footprint has a matching model.
- [FR05-S1-N-0-102.mod](FR05-S1-N-0-102.mod) and [MLPF-WB55-01E3.mod](MLPF-WB55-01E3.mod): older footprint-library exports alongside the `.pretty` versions.

The symbol names are `E73-2G4M08S1C`, `FR05-S1-N-0-102`, `JS203011JCQN`, `MLPF-WB55-01E3`, `NRF9160-SICA-R7`, `PCM12SMTR`, `SKRPADE010`, `STL6P3LLH6`, `STM32F042K6T6`, and `STM32WL55CCUx`.

## Install and use

```sh
git clone https://github.com/makeristic/Makeristic-KiCad-Library.git
```

1. Keep the checkout at a stable local path. In KiCad's symbol-library manager, add `Makeristic-KiCad-Library.lib` as a legacy library named `Makeristic-KiCad-Library`. A newer KiCad release may require importing or converting it into a new library; keep the original files when doing so.
2. In the footprint-library manager, add `Makeristic-KiCad-Library.pretty` with the same nickname. Choose a project-specific library table when sharing a board so that other contributors can reproduce the setup.
3. In **Preferences → Configure Paths**, define `MAKERISTIC_KICAD_LIBRARY` as the absolute path to this checkout. The footprint model paths use `${MAKERISTIC_KICAD_LIBRARY}/Makeristic-KiCad-Library.3dshapes/`, so they do not depend on the active board's directory.
4. Review each symbol's footprint assignment before updating a PCB. Seven symbols have exact assignments to the supplied footprints. `JS203011JCQN` has no supplied footprint, and `STM32F042K6T6` retains a wildcard because the collection offers three land-pattern variants; select the intended variant explicitly. `STM32WL55CCUx` references KiCad's standard `Package_DFN_QFN` library, which must be installed separately.

Two referenced models, `STMicroelectronics_-_STM32F042K6T6.step` and `STMicroelectronics_-_STL6P3LLH6.step`, are absent from this checkout. Missing 3D models affect rendering; inspect the actual pad geometry independently. No library-table files are supplied, so cloning alone does not register these libraries in KiCad.

## Status and verification

These are legacy KiCad library files collected in 2020. The [correctness audit](docs/code-audit-report.md) records import checks, footprint/model-reference fixes, and the datasheet-backed correction of the STM32WL55 exposed ground pin to pad 49. The files retain their legacy formats. Check pin numbering, pad dimensions, package variant, orientation, and manufacturer datasheets for every part before using it in a new design. Inclusion here does not establish electrical or manufacturing validation of a board.

Related designs include [Feather_STM32WB55](https://github.com/makeristic/Feather_STM32WB55) and [STUSB4500_Breakout](https://github.com/makeristic/STUSB4500_Breakout). The [Feather](https://github.com/makeristic/Adafruit_Feather) and [USB-C Feather](https://github.com/makeristic/Adafruit_Feather_USB-C) repositories provide board templates.

## Contributions

Report missing parts or incorrect mappings through [GitHub issues](https://github.com/makeristic/Makeristic-KiCad-Library/issues). Include the KiCad version, exact symbol/footprint name, datasheet revision, and a screenshot or reproduction. For new or imported assets, supply the original source and licence as well as dimensional and pinout references.

## Credits and licensing

- `E73-2G4M08S1C`: original credit to [GitWellBack's KiCad symbol contribution, pull request #1654](https://github.com/KiCad/kicad-symbols/pull/1654). That contribution also links its matching footprint submission.
- Several symbols include imported component-library metadata, and STEP headers identify additional model authors. The historical repository does not record a complete per-asset provenance or licence inventory.

A blanket MIT licence has not been added to this mixed-source collection. Existing third-party rights and source terms remain applicable; check the original provider's licence before redistributing an asset or library collection. When using data originating from KiCad libraries, consult the [KiCad library licence](https://www.kicad.org/libraries/license/) and its design-use exception. A complete provenance inventory is still needed before the collection can advertise a single redistribution licence.
