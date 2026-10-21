# Crafting Recipes

Shaped and shapeless grids for the blocks and items whose counts were not yet a pattern. Processing recipes stay in [RECIPE_DATA_GENERATION.md](RECIPE_DATA_GENERATION.md). Alloy, circuit, and installer counts stay in [MATERIAL_PROGRESSION.md](MATERIAL_PROGRESSION.md). Gear counts stay in [GEAR_MODULE_SYSTEM.md](GEAR_MODULE_SYSTEM.md) and [GADGETS.md](GADGETS.md).

A grid is three rows of three. `.` is empty. Tags are `c:` item tags. Every recipe makes the count in the output column. Higher tiers of cables, pipes, tubes, conductors, cubes, tanks, cells, and providers are the installer in [TIER_UPGRADING.md](TIER_UPGRADING.md), not a second craft.

## Normative constants

```normative
craft.hull.iron=4
craft.hull.velorite=2
craft.hull.machines=48
craft.hull.generators=20
craft.factory.alloy=3
craft.factory.recipes=12
craft.cable.output=8
craft.casing.input=4
craft.casing.output=8
craft.ethylene.bio_fuel=2
craft.ethylene.water_mb=10
craft.ethylene.hydrogen_mb=100
craft.ethylene.out_mb=100
craft.ethylene.ticks=100
craft.reaction.joules_per_tick=3000
```

## Patterns

| Name | Grid | Output |
| --- | --- | --- |
| `hull` | `I R I` / `V C V` / `I M I` | 1 |
| `corners` | `M . M` / `. . .` / `M . M` | 8 |
| `cross` | `. X .` / `X C X` / `. X .` | 1 |
| `row` | `A B C` on the middle row | the table's count |

`I` is an iron ingot. `V` is a velorite ingot. `C` is the tier circuit. `R` is redstone dust on a basic craft and that tier's alloy otherwise: infused, reinforced, atomic. `M` is the token in the machine table. The hull is 4 iron, 2 velorite, 1 circuit, 1 redstone or alloy, and 1 token.

## Machines and generators

Twelve machines at four tiers is 48 hull crafts. Five generators at four tiers is 20. The gas-burning generator uses the same hull. Its token is the electrolytic separator of the same tier, and the basic gas generator still uses the basic circuit and redstone dust.

| Token `M` | Blocks |
| --- | --- |
| `mekanika:velorite_dust` | `enrichment_chamber_<tier>` |
| `minecraft:flint` | `crusher_<tier>` |
| `minecraft:furnace` | `energized_smelter_<tier>` |
| `minecraft:coal` | `purification_chamber_<tier>` |
| `minecraft:gold_ingot` | `chemical_injection_chamber_<tier>` |
| `mekanika:velorite_block` | `chemical_dissolution_chamber_<tier>` |
| `minecraft:bucket` | `chemical_washer_<tier>` |
| `minecraft:diamond` | `chemical_crystallizer_<tier>` |
| `minecraft:redstone_block` | `metallurgic_infuser_<tier>` |
| `mekanika:sulfur_dust` | `chemical_oxidizer_<tier>` |
| `minecraft:lapis_lazuli` | `chemical_infuser_<tier>` |
| `minecraft:copper_block` | `electrolytic_separator_<tier>` |
| `minecraft:coal_block` | `heat_generator_<tier>` |
| `minecraft:glass` | `solar_generator_<tier>` |
| `minecraft:iron_bars` | `wind_generator_<tier>` |
| `minecraft:apple` | `bio_generator_<tier>` |
| `mekanika:electrolytic_separator_<tier>` | `gas_burning_generator_<tier>` |

A basic factory is the only factory craft. Higher factories are installers. The grid is `. A .` / `A M A` / `. C .`: 3 reinforced alloy, the basic machine, and 1 elite circuit. That is 12 recipes.

## Transmitters and storage

`row` recipes. Output 8. Tiers above basic are installers.

| Result, eight | Row |
| --- | --- |
| `universal_cable_basic` | velorite ingot, redstone dust, velorite ingot |
| `mechanical_pipe_basic` | copper ingot, redstone dust, copper ingot |
| `pressurized_tube_basic` | iron ingot, redstone dust, iron ingot |
| `thermodynamic_conductor_basic` | copper ingot, velorite ingot, copper ingot |

`cross` recipes. Output 1. `X` is the arm, `C` is the center.

| Result | Arms | Center |
| --- | --- | --- |
| `energy_cube_basic` | `universal_cable_basic` | `energy_tablet` |
| `induction_cell_basic` | `energy_cube_basic` | `basic_control_circuit` |
| `induction_provider_basic` | `universal_cable_basic` | `basic_control_circuit` |

The chemical tank is `V G V` / `G . G` / `V G V`: 4 velorite ingots and 4 glass. The fluid tank swaps the velorite for copper ingots.

## Casings

`corners` turns 4 of `M` into 8 blocks.

| Result | `M` |
| --- | --- |
| `dynamic_tank_casing` | iron ingot |
| `dynamic_tank_glass` | glass |
| `evaporation_casing` | copper ingot |
| `boiler_casing` | iron block |
| `sps_casing` | infused alloy |
| `induction_casing` | velorite block |
| `turbine_casing` | reinforced alloy |
| `fission_casing` | obsidian |
| `fusion_casing` | atomic alloy |

Shapeless, one casing of that structure plus one item, output 1.

| Result | Extra item |
| --- | --- |
| `dynamic_tank_valve`, `boiler_valve`, `evaporation_valve`, `turbine_valve` | bucket |
| `dynamic_tank_controller`, `evaporation_controller`, `boiler_controller` | basic control circuit |
| `sps_port` | bucket |
| `sps_controller` | advanced control circuit |
| `induction_port` | bucket |
| `induction_controller` | advanced control circuit |
| `induction_glass` | glass, shapeless with one induction casing |
| `turbine_controller` | elite control circuit |
| `turbine_vent` | iron bars |
| `fission_port` | bucket |
| `fission_controller` | elite control circuit |
| `fission_glass` | glass plus one fission casing |
| `fission_logic_adapter`, `boiler_logic_adapter` | redstone dust plus that structure's casing |
| `fusion_controller` | ultimate control circuit |
| `fusion_fuel_port` | bucket |
| `fusion_laser` | diamond |
| `fusion_logic_adapter` | redstone dust |
| `fusion_core` | atomic alloy plus one fusion casing |

Interior parts, shapeless, output 1 unless the count says otherwise.

| Result | Inputs | Count |
| --- | --- | --- |
| `superheating_element` | 4 copper ingots + 1 boiler casing | 1 |
| `pressure_disperser` | 4 iron bars + 1 boiler casing | 1 |
| `sps_coil` | 4 infused alloy + 1 advanced circuit | 1 |
| `turbine_blade` | 2 reinforced alloy | 2 |
| `turbine_coil` | 4 copper ingots + 1 reinforced alloy | 1 |
| `turbine_rotor` | 4 iron ingots + 1 turbine casing | 1 |
| `fission_fuel_assembly` | 4 velorite ingots + 1 elite circuit | 1 |
| `fission_control_rod` | 1 fuel assembly + 1 redstone dust | 1 |
| `evaporation_solar_panel` | 1 solar generator basic + 1 evaporation casing | 1 |

## Items already counted

These grids use the counts already fixed. They do not add ingredients.

| Result | Grid |
| --- | --- |
| `installer_advanced` | `cross` of infused alloy around an advanced circuit |
| `installer_elite` | `cross` of reinforced alloy around an elite circuit |
| `installer_ultimate` | `cross` of atomic alloy around an ultimate circuit |
| `energy_tablet` | `. C .` / `R G V` / `. C .` with `C` copper, `R` redstone, `G` gold, `V` velorite. Two copper, one of each other. Crafted empty |
| `upgrade_speed` | row: velorite dust, basic circuit, velorite dust |
| `upgrade_energy` | row: copper, basic circuit, copper |
| `upgrade_gas` | row: sulfur dust, basic circuit, sulfur dust |
| `upgrade_filter` | row: iron, basic circuit, iron |
| `upgrade_anchor` | row: obsidian, basic circuit, obsidian |
| `upgrade_muffling` | `. W .` / `W I W` / `. W .`, four wool and one iron ingot |
| Each module | row: atomic alloy, ultimate circuit, atomic alloy |
| Each MekaSuit piece and the Meka-Tool | `. A .` / `A T A` / `A C A`, four atomic alloy, one energy tablet, one ultimate circuit |
| `modification_station` | `A C A` / `A E A` / `. C .`, four atomic alloy, two ultimate circuits, one enrichment chamber |
| `configurator` | row: iron, basic circuit, iron |
| `dosimeter` | row: redstone, basic circuit, redstone |
| `free_runners` | row: infused alloy, empty, infused alloy |
| `atomic_disassembler` | `. A .` / `A T C` / `. . .`, two infused alloy, one energy tablet, one advanced circuit |
| `jetpack` | `. R .` / `R T E` / `. . .`, two reinforced alloy, one energy tablet, one elite circuit |
| Each lined piece except the chest | `cross` of reinforced alloy with an empty center |
| `lined_chest` | `cross` of reinforced alloy around an elite circuit |

Nuggets, ingots, and blocks stay at nine in and one out, and the reverse returns nine.

## QIO and the security desk

| Result | Grid |
| --- | --- |
| `qio_drive_basic` | `cross` of iron around a basic circuit |
| `qio_drive_advanced` | `cross` of infused alloy around an advanced circuit |
| `qio_drive_elite` | `cross` of reinforced alloy around an elite circuit |
| `qio_drive_ultimate` | `cross` of atomic alloy around an ultimate circuit |
| `qio_drive_array` | `. A .` / `A H A` / `. C .`, three atomic alloy, one chest, one ultimate circuit |
| `qio_dashboard` | `. G .` / `A C A` / `. T .`, one glass, two atomic alloy, one ultimate circuit, one energy tablet |
| `qio_importer` | the array grid with a hopper instead of the chest |
| `qio_exporter` | the array grid with a dropper instead of the chest |
| `security_desk` | `. I .` / `I H I` / `. C .`, three iron, one chest, one basic circuit |

## Ethylene

The public reaction is the amount used here. The block is `mekanika:pressurized_reaction_chamber`, one tier, no factory. The twelve processing names stay twelve.

Slots: 1 item in, 1 item out, 1 fluid in, 1 chemical in, 1 chemical out. Energy is 3,000 J/t. The screen is the machine screen with preset `reaction` in [GUI_LAYOUTS.md](GUI_LAYOUTS.md). The craft is the advanced hull: `R` is infused alloy, `C` is an advanced circuit, `M` is a bucket.

Bio fuel is `mekanika:bio_fuel`. The crusher turns 1 of each of these into 1 bio fuel: wheat, potato, carrot, beetroot, melon slice, pumpkin, sugar cane, apple. That is 8 recipes.

`mekanika:reacting`, 100 ticks, 300,000 J for the whole batch:

| Input | Output |
| --- | --- |
| 2 bio fuel, 10 mB water, 100 mB hydrogen | 100 mB `mekanika:ethylene` and 1 `mekanika:substrate` |

One hundred millibuckets of ethylene burned in a basic gas generator return `100 * 8000 = 800,000` J. The reaction spent 300,000 J.

## Worked-example index

```normative-examples
example.hull.iron=4
example.hull.velorite=2
example.hull.crafts=68
example.factory.alloy=3
example.factory.recipes=12
example.cable.output=8
example.casing.ratio=2
example.ethylene.bio_fuel=2
example.ethylene.water_mb=10
example.ethylene.hydrogen_mb=100
example.ethylene.out_mb=100
example.ethylene.joules=300000
example.ethylene.burn_j=800000
example.bio_foods=8
example.installer.alloy=4
example.meka.atomic=4
example.module.atomic=2
example.muffling.wool=4
```
