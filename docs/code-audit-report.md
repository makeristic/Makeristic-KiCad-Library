# KiCad library correctness audit

Date: 2026-10-26. Scope: the complete legacy symbol library, 12 footprints, two older footprint exports, model references, setup instructions, and provenance. Validation used KiCad CLI nightly 10.99.0 on temporary copies; no bulk format conversion was committed.

## Fixed findings

- **P1 — STM32WL55 ground pin could not map to the exposed copper pad.** In [Makeristic-KiCad-Library.lib](../Makeristic-KiCad-Library.lib#L630), `EXPOSED_PAD` used pin number `~`, while the selected standard QFN footprint has pad `49`. Both that pin and `VSSSMPS` pin 48 had output electrical types. They are now `VSS` pin 49 and `VSSSMPS` pin 48, both power inputs. [ST DS13293 Rev 5, table 20, page 60](https://www.st.com/resource/en/datasheet/stm32wl55cc.pdf#page=60) identifies pin 49 as the exposed VSS pad and pin 48 as VSSSMPS. The footprint and symbol positions are unchanged. Existing board symbol instances must be updated deliberately to receive library corrections.
- **P2 — Six default footprint IDs were bare names, wildcards, or did not name the supplied footprint.** FR05, MLPF, NRF9160, PCM12SMTR, SKRPADE010, and STL6P3LLH6 now identify their unique matching footprints through the `Makeristic-KiCad-Library` nickname. E73 already used an exact assignment. Every symbol pin in those seven mappings exists in its assigned footprint; geometry and pad numbering are unchanged.
- **P2 — Model lookup depended on the current working/project directory or an absent `eec.models` directory.** All ten model references in [Makeristic-KiCad-Library.pretty](../Makeristic-KiCad-Library.pretty/) now use the documented `MAKERISTIC_KICAD_LIBRARY` path variable. Six references resolve to supplied files. Four references still point to the two historically missing models listed below; no replacement model was invented.

## Remaining findings

- **P2 — Incomplete assets and default choices.** `JS203011JCQN` has no matching footprint in the collection. `STM32F042K6T6` offers Level A/B/C footprints, so its default remains a wildcard pending an explicit land-pattern choice. All three supplied variants cover its 32 symbol pins. Models `STMicroelectronics_-_STM32F042K6T6.step` and `STMicroelectronics_-_STL6P3LLH6.step` are absent. They require an authenticated source and licence before addition.
- **P2 — Component and RF suitability still require engineering review.** Pin-set agreement is not a manufacturer pinout or land-pattern certification. The STM32WL55 ground correction was checked against ST's datasheet; the remaining symbol pin functions, IPC variant dimensions, RF matching/antenna requirements, and 3D alignment have not all been independently validated against manufacturer drawings.
- **P2 — Mixed-source redistribution terms are incomplete.** The original README credits GitWellBack's E73 contribution; imported symbol metadata and STEP headers identify additional authors/providers. Existing notices are preserved. A blanket MIT licence remains inappropriate without a complete provenance inventory.

## Validation and reproduction

The audit first reproduced the missing pin-49 mapping with a failing assertion. After the correction, the STM32WL symbol contains exactly pins 1–49, with pins 48 and 49 marked as power inputs. Automated checks also confirmed the seven exact local footprint IDs and their complete pin-to-pad coverage, all three STM32F042 variant pin sets, and the six available versus two missing unique model files.

The following commands ran in `/tmp/makeristic-library-template-audit/`, with the repository copied there and `XDG_CONFIG_HOME` set to a temporary `config` directory:

```sh
kicad-cli-nightly sym upgrade --output Makeristic-KiCad-Library.kicad_sym Makeristic-KiCad-Library.lib
kicad-cli-nightly sym export svg --output symbol-svg Makeristic-KiCad-Library.kicad_sym
kicad-cli-nightly fp export svg --output footprint-svg Makeristic-KiCad-Library.pretty
```

All ten symbols loaded and produced 12 unit SVGs; all 12 footprints produced SVGs. The output directories were created before export. Direct SVG export from the legacy `.lib` produced no files despite returning success, so conversion and artifact counts were checked explicitly. The modern converted symbol file and renderings remain temporary audit artifacts.

`git diff --check` and relative documentation-link checks passed. These checks establish parsing and reference consistency, not a completed hardware validation or a clean manufacturing release.
