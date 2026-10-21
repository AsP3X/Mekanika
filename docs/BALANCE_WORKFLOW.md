# Balance Workflow

Where each published number lives, and which file wins when two sources disagree.

## Normative constants

```normative
balance.version=1
balance.recipe_overrides_config=true
balance.reload_energy_ticks=true
balance.reload_worldgen=false
```

`balance.version` is written into the config header. Recipe JSON does not carry it. A newer config version than the build logs an error and keeps the previous loaded values.

## Who owns the number

| Number | Owner | Key or file |
| --- | --- | --- |
| Machine J/t, batch ticks, tiers, upgrades, cubes, cables, tanks | [POWER_BUDGET.md](POWER_BUDGET.md) | `machine.*`, `upgrade.*`, `energy.*`, `chemical.tank.*`, `chemical.pipe.*` |
| Ore item counts and chemical millibuckets | [ORE_PROCESSING_CHAIN.md](ORE_PROCESSING_CHAIN.md) | `data/mekanika/recipe/` |
| Alloy and circuit counts | [MATERIAL_PROGRESSION.md](MATERIAL_PROGRESSION.md) | `data/mekanika/recipe/` |
| Ore veins and Fortune | [WORLD_GENERATION.md](WORLD_GENERATION.md) | `worldgen.*` |
| Generator J/t and fuels | [GENERATORS_REACTORS.md](GENERATORS_REACTORS.md) | `generator.*` and `data/mekanika/heat_fuel/` |
| Conductor capacity and leak | [HEAT_SYSTEM.md](HEAT_SYSTEM.md) | `conductor.*` |
| Multiblock formulas | [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md) | `tank.*`, `evap.*`, `boiler.*`, `sps.*`, `matrix.*`, `turbine.*`, `fission.*`, `fusion.*` |

The key catalog with types and clamps is [CONFIG_STRUCTURE.md](CONFIG_STRUCTURE.md).

## Precedence

A data pack recipe replaces the matching recipe id. It does not change `machine.enrichment.joules_per_tick`. Config reloads energy, ticks, upgrade caps, cable throughput, and tank capacity without a restart. Worldgen, recipes, and block registration do not reload until the next launch.

Missing config keys use the normative defaults. A value outside the clamp in the config doc is rejected and the default is kept. The log line names the key and the rejected value.

## Change steps

1. Edit the normative block in the owner doc and the matching key in the config doc in the same change.
2. If the number is a recipe amount, edit the JSON example in [RECIPE_DATA_GENERATION.md](RECIPE_DATA_GENERATION.md) as well.
3. Recompute the worked-example index in the owner doc. The ore energy totals in the power budget must still match the operation counts.
4. Bump `balance.version` when a default changes.

## Worked check

Enrichment is 1,000 J/t in the power budget and in the config catalog. One basic heat generator is also 1,000 J/t in the generator doc. One coal in that generator lasts 1,600 ticks, so it finishes 8 enrichment batches (8 × 200 ticks) if nothing else draws power.
