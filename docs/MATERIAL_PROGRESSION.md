# Material Progression

Crafting counts for the primary metal, the three alloys, and the four circuits. Machine energy is in [POWER_BUDGET.md](POWER_BUDGET.md). Ore yields are in [ORE_PROCESSING_CHAIN.md](ORE_PROCESSING_CHAIN.md).

The alloy and circuit counts match Mekanism's public metallurgic and circuit recipes. The metal id is Mekanika's: velorite, in the osmium-style slot. Common tag `c:ingots/velorite`. Ore tag `c:ores/velorite`, plus the compatibility tag `c:ores/osmium_style` already named for worldgen.

## Normative constants

```normative
material.nuggets_per_ingot=9
material.ingots_per_block=9
material.infused.redstone_mb=10
material.reinforced.diamond_mb=20
material.atomic.refined_obsidian_mb=40
material.circuit.basic.redstone_mb=20
material.circuit.upgrade.alloys=2
material.infusion.redstone_mb_per_dust=10
material.infusion.diamond_mb_per_gem=20
material.infusion.refined_obsidian_mb_per_dust=40
material.tablet.capacity=100000
```

## Items

| Id | English |
| --- | --- |
| `mekanika:velorite_ore` | Velorite Ore |
| `mekanika:deepslate_velorite_ore` | Deepslate Velorite Ore |
| `mekanika:raw_velorite` | Raw Velorite |
| `mekanika:velorite_ingot` | Velorite Ingot |
| `mekanika:velorite_nugget` | Velorite Nugget |
| `mekanika:velorite_dust` | Velorite Dust |
| `mekanika:velorite_block` | Velorite Block |
| `mekanika:infused_alloy` | Infused Alloy |
| `mekanika:reinforced_alloy` | Reinforced Alloy |
| `mekanika:atomic_alloy` | Atomic Alloy |
| `mekanika:basic_control_circuit` | Basic Control Circuit |
| `mekanika:advanced_control_circuit` | Advanced Control Circuit |
| `mekanika:elite_control_circuit` | Elite Control Circuit |
| `mekanika:ultimate_control_circuit` | Ultimate Control Circuit |
| `mekanika:energy_tablet` | Energy Tablet |
| `mekanika:sulfur_dust` | Sulfur Dust |

Infusion chemicals: `mekanika:redstone`, `mekanika:diamond`, `mekanika:refined_obsidian`. One redstone dust enriches to 10 mB redstone. One diamond enriches to 20 mB diamond. One refined obsidian dust enriches to 40 mB refined obsidian. Each enrichment is one 200-tick enrichment batch.

## Alloys

Metallurgic infuser, 1,000 J/t, 200 ticks.

| Output | Item | Chemical |
| --- | --- | --- |
| 1 infused alloy | 1 copper ingot | 10 mB redstone |
| 1 reinforced alloy | 1 infused alloy | 20 mB diamond |
| 1 atomic alloy | 1 reinforced alloy | 40 mB refined obsidian |

One redstone dust is exactly one infused alloy. One diamond is exactly one reinforced alloy. One refined obsidian dust is exactly one atomic alloy.

## Circuits

| Output | Recipe |
| --- | --- |
| 1 basic | Infuser: 1 velorite ingot + 20 mB redstone |
| 1 advanced | Shaped row: infused alloy, basic circuit, infused alloy |
| 1 elite | Shaped row: reinforced alloy, advanced circuit, reinforced alloy |
| 1 ultimate | Shaped row: atomic alloy, elite circuit, atomic alloy |

The shaped row uses two alloys and one circuit of the previous tier.

## Where tiers start

| Tier | Gate | Machines and storage |
| --- | --- | --- |
| Basic | Velorite ingot and a basic circuit | Enrichment, crusher, smelter, infuser, basic cable, basic cube, heat generator |
| Advanced | Infused alloy and an advanced circuit | Purification, injection, electrolytic separator, chemical infuser |
| Elite | Reinforced alloy and an elite circuit | Dissolution, washer, crystallizer, basic factory |
| Ultimate | Atomic alloy and an ultimate circuit | Ultimate machines, reactors, QIO blocks |

Tier installers, one shaped craft:

| Installer | Ingredients |
| --- | --- |
| Advanced | 4 infused alloy + 1 advanced circuit |
| Elite | 4 reinforced alloy + 1 elite circuit |
| Ultimate | 4 atomic alloy + 1 ultimate circuit |

Applying an installer keeps contents, side configuration, upgrades, and redstone mode, as in [TIER_UPGRADING.md](TIER_UPGRADING.md). A factory's slot count becomes the number in the power budget for the new tier.

## Basic machine crafts

Each shaped craft makes one machine. The hull grid is in [CRAFTING_RECIPES.md](CRAFTING_RECIPES.md): 4 iron ingots, 2 velorite ingots, 1 circuit, 1 redstone dust or that tier's alloy, and 1 machine token.

The energy tablet holds 100,000 J. Its craft is 2 copper ingots, 1 gold ingot, 1 redstone dust, and 1 velorite ingot. It is crafted empty.

Nine nuggets make one ingot. Nine ingots make one block. The reverse crafts return 9.

## Worked-example index

```normative-examples
example.alloy.infused.redstone_dust=1
example.alloy.reinforced.diamonds=1
example.alloy.atomic.refined_obsidian_dust=1
example.circuit.basic.redstone_dust=2
example.circuit.advanced.infused_alloys=2
example.tablet.capacity=100000
```
