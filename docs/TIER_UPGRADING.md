# Tier Installers

Use an installer on a block. The block becomes the next tier and keeps its contents. There is no installer slot on the modification station, and the use does not open a screen.

The public installer is used on the block, consumes one item, and does nothing while a player has that block's screen open. Mekanika uses that gesture. The crafts are in [MATERIAL_PROGRESSION.md](MATERIAL_PROGRESSION.md).

## Normative constants

```normative
installer.items=3
installer.families=38
installer.steps=3
installer.consume=1
installer.creative_consume=0
```

## Items

| Id | Upgrades |
| --- | --- |
| `mekanika:installer_advanced` | Basic to advanced |
| `mekanika:installer_elite` | Advanced to elite |
| `mekanika:installer_ultimate` | Ultimate is the last tier. This item upgrades elite to ultimate |

English names are "Advanced Installer", "Elite Installer", and "Ultimate Installer".

## Use

The player uses the installer on a block. Sneak is not required. The block's own use does not run, so the screen stays closed.

Checks, in order. The first failure stops the use. The installer is not consumed.

| Check | Result | English |
| --- | --- | --- |
| The block is not in the family list | No change | "That block cannot be upgraded." |
| The player fails the block's security check | No change | "Access denied." |
| Any player has that block's screen open | No change | "Close the screen before upgrading." |
| The block's tier is not exactly one step below the installer | No change | "That block is not one tier below this installer." |
| The new tier needs proxy cells that are not air and are not already this anchor's proxies | No change | "The extra cells are not air." |
| Otherwise | The block id gains the new tier. Survival consumes one installer. Creative consumes zero | "Upgraded to %s." |

`%s` is Advanced, Elite, or Ultimate.

One use changes one block. Neighbors on a cable, tube, pipe, or conductor stay at their old tier.

## Families

Thirty-eight families. Each has the four suffixes `basic`, `advanced`, and `elite`, then `ultimate`. Only the first three accept an installer.

| Count | Ids |
| --- | --- |
| 12 | The processing machines in [MACHINE_IO.md](MACHINE_IO.md) |
| 12 | `<machine>_factory_<tier>` for those machines |
| 7 | `energy_cube`, `chemical_tank`, `fluid_tank`, `universal_cable`, `pressurized_tube`, `mechanical_pipe`, `thermodynamic_conductor` |
| 5 | `heat_generator`, `solar_generator`, `wind_generator`, `bio_generator`, `gas_burning_generator` |
| 2 | `induction_cell`, `induction_provider` |

A charge pad, a modification station, a waste barrel, a QIO drive, and a multiblock casing are not in the list. `12 + 12 + 7 + 5 + 2 = 38`.

## What stays

The new block keeps side configuration, redstone mode, security, upgrades, items, energy, fluids, chemicals, heat, and factory `mekanika:slot_progress`. New factory slots are appended at progress 0. A basic factory has 3 slots. An advanced installer makes it 5. The first three progress values stay.

Machine progress is the stored integer. The new tier's tick length is `base_ticks / machine.tier_speed`, from [POWER_BUDGET.md](POWER_BUDGET.md). If the stored progress is already at least that length, the slot finishes on the next tick that is allowed to run. The use itself does not finish the batch.

A basic enrichment slot at progress 100 has 200 ticks. Advanced speed is 2, so the new length is 100. Progress stays 100. The next allowed tick finishes the batch. Progress 50 stays 50 and does not finish on that next tick.

A generator keeps its remaining fuel ticks. The new tier's multiplier applies on the next production tick.

A cable, tube, pipe, tank, or conductor keeps its buffer. These capacities grow, so a basic cable at 1,000 J becomes an advanced cable at 1,000 J in a buffer of 8,000 J. A shrink that does delete contents uses the message already named in [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md) or [HEAT_SYSTEM.md](HEAT_SYSTEM.md).

An induction cell or provider inside a matrix changes that one block. The structure is marked dirty and reforms. The controller keeps the stored energy. The new capacity and rate are the sums for the new tier.

An advanced solar generator has no proxy cells. The elite installer claims the ten cells in [OVERSIZED_BLOCKS.md](OVERSIZED_BLOCKS.md). If any of those cells is blocked, the basic rule above fails and the advanced generator stays. A wind generator already has its four proxies at every tier, so an upgrade keeps those proxies.

## Worked-example index

```normative-examples
example.families=38
example.installers=3
example.steps=3
example.survival.consumed=1
example.creative.consumed=0
example.basic_advanced=1
example.basic_elite=0
example.ultimate=0
example.factory.slots=5
example.progress.kept=100
example.progress.ticks=100
example.progress.completes=1
example.progress.stays=50
example.cable.kept=1000
example.one_block=1
```
