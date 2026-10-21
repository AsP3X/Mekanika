# Config Structure

Server config keys for the numbers owned by the balance docs. Recipe amounts stay in data packs. See [BALANCE_WORKFLOW.md](BALANCE_WORKFLOW.md) for which file wins.

Format is TOML. One file per group: `mekanika-general.toml`, `mekanika-energy.toml`, `mekanika-machines.toml`, `mekanika-chemicals.toml`, `mekanika-generators.toml`, `mekanika-multiblock.toml`, `mekanika-worldgen.toml`, `mekanika-client.toml`. Synced groups are general, energy, machines, chemicals, generators, multiblock, and worldgen. Client stays local.

`balance.version` in the general file starts at 1.

## General

| Key | Type | Default | Clamp |
| --- | --- | --- | --- |
| `general.debug_mode` | bool | false | |
| `general.default_security` | string | public | public, private, trusted |
| `general.operator_bypass` | bool | false | |
| `general.creative_bypass` | bool | false | |
| `balance.version` | int | 1 | 1 or greater |

## Machines, upgrades, energy, chemicals

The defaults are the normative block in [POWER_BUDGET.md](POWER_BUDGET.md). Types are integers. Joules and millibuckets clamp from 0 to `9223372036854775807`. Counts clamp from 1 to 64. Speed multipliers clamp from 1 to 64.

Keys:

- `machine.enrichment.joules_per_tick` = 1000
- `machine.crusher.joules_per_tick` = 1600
- `machine.smelter.joules_per_tick` = 1000
- `machine.purification.joules_per_tick` = 2000
- `machine.injection.joules_per_tick` = 3000
- `machine.dissolution.joules_per_tick` = 3200
- `machine.washer.joules_per_tick` = 1000
- `machine.crystallizer.joules_per_tick` = 3000
- `machine.infuser.joules_per_tick` = 1000
- `machine.oxidizer.joules_per_tick` = 3000
- `machine.chemical_infuser.joules_per_tick` = 3000
- `machine.electrolytic_separator.joules_per_tick` = 1000
- `machine.reaction.joules_per_tick` = 3000
- `machine.reaction.ticks` = 100
- `machine.base_ticks` = 200
- `machine.energy_buffer_ticks` = 20
- `machine.tier_speed.basic` = 1
- `machine.tier_speed.advanced` = 2
- `machine.tier_speed.elite` = 4
- `machine.tier_speed.ultimate` = 8
- `factory.slots.basic` = 3
- `factory.slots.advanced` = 5
- `factory.slots.elite` = 7
- `factory.slots.ultimate` = 9
- `upgrade.speed.max` = 8
- `upgrade.energy.max` = 8
- `upgrade.gas.max` = 8
- `upgrade.muffling.max` = 4
- `upgrade.filter.max` = 1
- `upgrade.anchor.max` = 1
- `upgrade.energy_relief_per_count` = 10
- `energy.cube.basic.capacity` = 1000000
- `energy.cube.advanced.capacity` = 8000000
- `energy.cube.elite.capacity` = 64000000
- `energy.cube.ultimate.capacity` = 512000000
- `energy.cable.basic.throughput` = 1000
- `energy.forge.fe_numerator` = 5
- `energy.forge.joule_denominator` = 2
- `energy.cable.advanced.throughput` = 8000
- `energy.cable.elite.throughput` = 32000
- `energy.cable.ultimate.throughput` = 128000
- `chemical.tank.basic.capacity` = 16000
- `chemical.tank.advanced.capacity` = 64000
- `chemical.tank.elite.capacity` = 256000
- `chemical.tank.ultimate.capacity` = 1024000
- `chemical.pipe.basic.throughput` = 250
- `chemical.pipe.advanced.throughput` = 1000
- `chemical.pipe.elite.throughput` = 4000
- `chemical.pipe.ultimate.throughput` = 16000
- `chemical.tank.basic.throughput` = 250
- `chemical.tank.advanced.throughput` = 4000
- `chemical.tank.elite.throughput` = 32000
- `chemical.tank.ultimate.throughput` = 64000
- `buffer.chemical.basic` = 4000
- `buffer.chemical.advanced` = 16000
- `buffer.chemical.elite` = 256000
- `buffer.chemical.ultimate` = 1024000
- `buffer.fluid.basic` = 2000
- `buffer.fluid.advanced` = 8000
- `buffer.fluid.elite` = 32000
- `buffer.fluid.ultimate` = 128000
- `fluid.pipe.basic.throughput` = 250
- `fluid.pipe.advanced.throughput` = 1000
- `fluid.pipe.elite.throughput` = 8000
- `fluid.pipe.ultimate.throughput` = 32000
- `fluid.tank.basic.capacity` = 32000
- `fluid.tank.advanced.capacity` = 64000
- `fluid.tank.elite.capacity` = 128000
- `fluid.tank.ultimate.capacity` = 256000
- `fluid.tank.basic.throughput` = 1000
- `fluid.tank.advanced.throughput` = 4000
- `fluid.tank.elite.throughput` = 16000
- `fluid.tank.ultimate.throughput` = 64000

A cable buffer is `energy.cable.<tier>.throughput`. Do not add a second buffer key for cables.

## Generators, heat, multiblocks

Generator and conductor defaults stay in [GENERATORS_REACTORS.md](GENERATORS_REACTORS.md) and [HEAT_SYSTEM.md](HEAT_SYSTEM.md). Multiblock defaults stay in [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md). Those normative keys are copied into `mekanika-generators.toml` and `mekanika-multiblock.toml` under the same names.

`generator.heat.output` is the same value as `generator.heat.energy_per_tick` (1000). Fission heat cap is `fission.max_heat_per_assembly` (20000), not a temperature key.

## World

| Key | Type | Default | Clamp |
| --- | --- | --- | --- |
| `worldgen.velorite.enabled` | bool | true | |
| `worldgen.velorite.vein_size` | int | 8 | 1–64 |
| `worldgen.velorite.veins_per_chunk` | int | 6 | 0–64 |
| `worldgen.velorite.min_y` | int | -32 | -64–320 |
| `worldgen.velorite.max_y` | int | 48 | -64–320 |
| `worldgen.sulfur.enabled` | bool | true | |
| `worldgen.sulfur.vein_size` | int | 6 | 1–64 |
| `worldgen.sulfur.veins_per_chunk` | int | 4 | 0–64 |
| `worldgen.sulfur.min_y` | int | 0 | -64–320 |
| `worldgen.sulfur.max_y` | int | 96 | -64–320 |
| `worldgen.salt.enabled` | bool | false | |
| `worldgen.salt.vein_size` | int | 6 | 1–64 |
| `worldgen.salt.veins_per_chunk` | int | 3 | 0–64 |
| `worldgen.salt.min_y` | int | 40 | -64–320 |
| `worldgen.salt.max_y` | int | 80 | -64–320 |
| `worldgen.fortune.extra_per_level` | int | 1 | 0–4 |
| `worldgen.fortune.max_drops` | int | 4 | 1–16 |

`min_y` must be less than or equal to `max_y`. If it is not, both revert to the defaults.

## QIO

Integer keys from [QIO_SYSTEM.md](QIO_SYSTEM.md). Item rates clamp from 1 to 64. Joules per item clamp from 0 upward.

| Key | Default |
| --- | --- |
| `qio.transfer.items_per_tick` | 8 |
| `qio.transfer.joules_per_item` | 100 |
| `qio.transfer.max_items_per_tick` | 64 |
| `qio.filter.max` | 16 |
| `qio.drive.basic.count` | 16000 |
| `qio.drive.basic.types` | 128 |
| `qio.drive.advanced.count` | 128000 |
| `qio.drive.advanced.types` | 256 |
| `qio.drive.elite.count` | 1048000 |
| `qio.drive.elite.types` | 1024 |
| `qio.drive.ultimate.count` | 16000000000 |
| `qio.drive.ultimate.types` | 8192 |

## Client

| Key | Type | Default | Clamp |
| --- | --- | --- | --- |
| `client.particles.enabled` | bool | true | |
| `client.sounds.volume` | float | 1.0 | 0–1 |

## Sample

```toml
[machine.enrichment]
joules_per_tick = 1000
```

That TOML path is the key `machine.enrichment.joules_per_tick`.
