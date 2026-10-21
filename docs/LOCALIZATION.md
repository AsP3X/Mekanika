# Localization for Mekanika

This document defines naming and language key conventions. All content is original and conceptual.

## Goals

- Consistent, clear names for all blocks, items, and messages.
- Support for multiple languages later.
- Easy for Claude and Grok to follow.
- No trademarked names from other mods.

## Key Prefixes

- Blocks: `block.mekanika.<id>`
- Items: `item.mekanika.<id>`
- Tooltips: `tooltip.mekanika.<id>`
- Messages: `message.mekanika.<category>.<id>`
- GUIs: `gui.mekanika.<screen>.<element>`
- Configs: `config.mekanika.<key>` (optional)

## Naming Style

- Clear, descriptive, industrial tone.
- Tier names: Basic, Advanced, Elite, Ultimate.
- Avoid exact copies of other mods’ names.
- Consistent capitalization.

## Examples

- `block.mekanika.enrichment_chamber` → "Enrichment Chamber"
- `item.mekanika.infused_alloy` → "Infused Alloy"
- `message.mekanika.multiblock.not_cuboid` → "Structure is not a rectangular shell at %s"
- `gui.mekanika.side_config.title` → "Side Configuration"

## English inventory

Block and item names use `block.mekanika.<id>` and `item.mekanika.<id>`. A block item uses the block's English line. Multiblock block English is the registry table in [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md). Material English is the item table in [MATERIAL_PROGRESSION.md](MATERIAL_PROGRESSION.md). Gear, gadgets, QIO, generators, and transporters use the names in those docs. This list is the processing set and the messages that are not already in those tables.

| Id | English |
| --- | --- |
| `enrichment_chamber` | Enrichment Chamber |
| `crusher` | Crusher |
| `energized_smelter` | Energized Smelter |
| `purification_chamber` | Purification Chamber |
| `chemical_injection_chamber` | Chemical Injection Chamber |
| `chemical_dissolution_chamber` | Chemical Dissolution Chamber |
| `chemical_washer` | Chemical Washer |
| `chemical_crystallizer` | Chemical Crystallizer |
| `metallurgic_infuser` | Metallurgic Infuser |
| `chemical_oxidizer` | Chemical Oxidizer |
| `chemical_infuser` | Chemical Infuser |
| `electrolytic_separator` | Electrolytic Separator |
| `modification_station` | Modification Station |
| `charge_pad` | Charge Pad |
| `waste_barrel` | Waste Barrel |
| `network_reader` | Network Reader |
| `energy_cube_basic` | Basic Energy Cube |
| `energy_cube_advanced` | Advanced Energy Cube |
| `energy_cube_elite` | Elite Energy Cube |
| `energy_cube_ultimate` | Ultimate Energy Cube |
| `chemical_tank_basic` | Basic Chemical Tank |
| `chemical_tank_advanced` | Advanced Chemical Tank |
| `chemical_tank_elite` | Elite Chemical Tank |
| `chemical_tank_ultimate` | Ultimate Chemical Tank |
| `fluid_tank_basic` | Basic Fluid Tank |
| `fluid_tank_advanced` | Advanced Fluid Tank |
| `fluid_tank_elite` | Elite Fluid Tank |
| `fluid_tank_ultimate` | Ultimate Fluid Tank |
| `universal_cable_basic` | Basic Universal Cable |
| `universal_cable_advanced` | Advanced Universal Cable |
| `universal_cable_elite` | Elite Universal Cable |
| `universal_cable_ultimate` | Ultimate Universal Cable |
| `pressurized_tube_basic` | Basic Pressurized Tube |
| `pressurized_tube_advanced` | Advanced Pressurized Tube |
| `pressurized_tube_elite` | Elite Pressurized Tube |
| `pressurized_tube_ultimate` | Ultimate Pressurized Tube |
| `mechanical_pipe_basic` | Basic Mechanical Pipe |
| `mechanical_pipe_advanced` | Advanced Mechanical Pipe |
| `mechanical_pipe_elite` | Elite Mechanical Pipe |
| `mechanical_pipe_ultimate` | Ultimate Mechanical Pipe |

Factory English is the tier name, the machine English, and the word Factory. `enrichment_chamber_factory_basic` is "Basic Enrichment Chamber Factory".

Messages whose English is already fixed stay in the system doc that wrote them. The keys are:

| Key | English |
| --- | --- |
| `message.mekanika.qio.frequency_exists` | That frequency already exists. |
| `message.mekanika.qio.denied` | You cannot use this frequency. |
| `message.mekanika.debug.denied` | Debug tools are disabled. |
| `message.mekanika.debug.no_target` | No Mekanika block in sight. |
| `message.mekanika.heat.heat_voided` | Stored heat exceeded the new capacity and the extra was removed |
| `message.mekanika.transmitter.contents_voided` | Stored contents exceeded the new capacity and the extra was removed |
| `message.mekanika.radiation.sick` | Radiation sickness. |
| `message.mekanika.radiation.recovering` | Radiation is falling. |
| `gui.mekanika.machine.energy` | Energy |
| `gui.mekanika.machine.progress` | Progress |
| `gui.mekanika.machine.upgrades` | Upgrades |
| `gui.mekanika.side_config.title` | Side Configuration |
| `gui.mekanika.status.running` | Running |
| `gui.mekanika.status.idle` | Idle |
| `gui.mekanika.security.public` | Public |
| `gui.mekanika.security.private` | Private |
| `gui.mekanika.security.trusted` | Trusted |
| `gui.mekanika.security.unknown` | Unknown player. |
| `gui.mekanika.factory.auto_sort` | Auto-sort |
| `gui.mekanika.tank.dump` | Dump |
| `gui.mekanika.redstone.ignored` | Ignored |
| `gui.mekanika.redstone.high` | High |
| `gui.mekanika.redstone.low` | Low |
| `gui.mekanika.redstone.pulse` | Pulse |
| `block.mekanika.fission_logic_adapter` | Fission Logic Adapter |
| `block.mekanika.fusion_logic_adapter` | Fusion Logic Adapter |
| `block.mekanika.boiler_logic_adapter` | Boiler Logic Adapter |
| `item.mekanika.installer_advanced` | Advanced Installer |
| `item.mekanika.installer_elite` | Elite Installer |
| `item.mekanika.installer_ultimate` | Ultimate Installer |
| `item.mekanika.upgrade_speed` | Speed Upgrade |
| `item.mekanika.upgrade_energy` | Energy Upgrade |
| `item.mekanika.upgrade_gas` | Gas Upgrade |
| `item.mekanika.upgrade_muffling` | Muffling Upgrade |
| `item.mekanika.upgrade_filter` | Filter Upgrade |
| `item.mekanika.upgrade_anchor` | Anchor Upgrade |
| `item.mekanika.bio_fuel` | Bio Fuel |
| `item.mekanika.substrate` | Substrate |
| `block.mekanika.pressurized_reaction_chamber` | Pressurized Reaction Chamber |
| `message.mekanika.installer.unsupported` | That block cannot be upgraded. |
| `message.mekanika.installer.screen_open` | Close the screen before upgrading. |
| `message.mekanika.installer.wrong_tier` | That block is not one tier below this installer. |
| `message.mekanika.installer.blocked` | The extra cells are not air. |
| `message.mekanika.installer.done` | Upgraded to %s. |

Multiblock failure English is the table in the structure spec. Do not add `message.mekanika.multiblock.invalid_shape`. The code is `not_cuboid`.

## Normative counts

```normative
lang.processing_names=12
lang.factory_pattern=tier_machine_factory
lang.gui.status.running=Running
```

## Worked-example index

```normative-examples
example.lang.enrichment=Enrichment Chamber
example.lang.factory_basic_enrichment=Basic Enrichment Chamber Factory
example.lang.not_cuboid=Structure is not a rectangular shell at %s
example.lang.processing_names=12
```


## Implementation Notes

- All user-facing strings use lang keys.
- Provide an English (`en_us`) file as the base.
- Document new keys when adding content.
- Support data pack lang overrides.
- Test for missing translations (fallback to key or English).

## Edge Cases

- Very long names in GUIs.
- Pluralization if needed.
- Dynamic values in messages (use format placeholders).

Consistent localization keeps the mod professional and translatable.
