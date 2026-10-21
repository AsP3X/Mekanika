# Progression and Power Budget

Defaults that tie machines to the generators in [GENERATORS_REACTORS.md](GENERATORS_REACTORS.md). Ore counts are in [ORE_PROCESSING_CHAIN.md](ORE_PROCESSING_CHAIN.md). Crafting counts are in [MATERIAL_PROGRESSION.md](MATERIAL_PROGRESSION.md).

Mekanism's public machine-usage config draws 5 J/t for an enrichment chamber and 8 J/t for a crusher, up to 16 J/t for dissolution. Mekanika keeps those ratios and sets enrichment to 1,000 J/t, which is the basic heat generator's output. The scale factor is 200. Operation time for a batch machine is 200 ticks, the public base of 10 seconds.

## Normative constants

```normative
machine.scale_from_public_usage=200
machine.base_ticks=200
machine.energy_buffer_ticks=20

machine.enrichment.joules_per_tick=1000
machine.crusher.joules_per_tick=1600
machine.smelter.joules_per_tick=1000
machine.purification.joules_per_tick=2000
machine.injection.joules_per_tick=3000
machine.dissolution.joules_per_tick=3200
machine.washer.joules_per_tick=1000
machine.crystallizer.joules_per_tick=3000
machine.infuser.joules_per_tick=1000
machine.oxidizer.joules_per_tick=3000
machine.chemical_infuser.joules_per_tick=3000
machine.electrolytic_separator.joules_per_tick=1000
machine.reaction.joules_per_tick=3000
machine.reaction.ticks=100

machine.tier_speed.basic=1
machine.tier_speed.advanced=2
machine.tier_speed.elite=4
machine.tier_speed.ultimate=8

factory.slots.basic=3
factory.slots.advanced=5
factory.slots.elite=7
factory.slots.ultimate=9

upgrade.speed.max=8
upgrade.energy.max=8
upgrade.gas.max=8
upgrade.muffling.max=4
upgrade.filter.max=1
upgrade.anchor.max=1
upgrade.energy_relief_per_count=10

energy.cube.basic.capacity=1000000
energy.cube.advanced.capacity=8000000
energy.cube.elite.capacity=64000000
energy.cube.ultimate.capacity=512000000

energy.cable.basic.throughput=1000
energy.forge.fe_numerator=5
energy.forge.joule_denominator=2
energy.cable.advanced.throughput=8000
energy.cable.elite.throughput=32000
energy.cable.ultimate.throughput=128000

chemical.tank.basic.capacity=16000
chemical.tank.advanced.capacity=64000
chemical.tank.elite.capacity=256000
chemical.tank.ultimate.capacity=1024000
chemical.pipe.basic.throughput=250
chemical.pipe.advanced.throughput=1000
chemical.pipe.elite.throughput=4000
chemical.pipe.ultimate.throughput=16000

chemical.tank.basic.throughput=250
chemical.tank.advanced.throughput=4000
chemical.tank.elite.throughput=32000
chemical.tank.ultimate.throughput=64000

buffer.chemical.basic=4000
buffer.chemical.advanced=16000
buffer.chemical.elite=256000
buffer.chemical.ultimate=1024000
buffer.fluid.basic=2000
buffer.fluid.advanced=8000
buffer.fluid.elite=32000
buffer.fluid.ultimate=128000
fluid.pipe.basic.throughput=250
fluid.pipe.advanced.throughput=1000
fluid.pipe.elite.throughput=8000
fluid.pipe.ultimate.throughput=32000
fluid.tank.basic.capacity=32000
fluid.tank.advanced.capacity=64000
fluid.tank.elite.capacity=128000
fluid.tank.ultimate.capacity=256000
fluid.tank.basic.throughput=1000
fluid.tank.advanced.throughput=4000
fluid.tank.elite.throughput=16000
fluid.tank.ultimate.throughput=64000
```

Each cube tier holds 8 times the previous tier. Ultimate is 512,000,000 J. A basic induction cell is 1,000,000,000 J, so one basic cell holds as much as 1,000 basic cubes, and more than one ultimate cube.

## How a machine spends energy

Batch machines (enrichment, crusher, smelter, purification, injection, dissolution, crystallizer, infuser, oxidizer) finish one recipe every `machine.base_ticks` ticks at basic tier. They draw their joules-per-tick the whole time. The buffer holds `joules_per_tick * machine.energy_buffer_ticks`.

Flow machines (washer, electrolytic separator, chemical infuser) draw every tick they move chemicals. Their per-tick amounts are in the ore and material docs.

A tier installer multiplies speed and joules per tick by `machine.tier_speed`. Ticks become `base_ticks / speed`, integer division, at least 1. Joules per operation stay the same.

A factory adds slots. Each active slot draws the full per-tick cost. Basic enrichment factory with 3 slots draws 3,000 J/t.

Upgrades, applied after tier:

- Speed count `s`: divide ticks by `(1 + s)` and multiply joules per tick by `(1 + s)`.
- Energy count `e`: joules per tick become `joules * 100 / (100 + 10 * e)`.
- Gas count `g`: chemical and fluid millibuckets become `amount * 100 / (100 + 10 * g)`, at least 1 when the base amount is at least 1.

Eight speed upgrades on a basic enrichment chamber: 200 / 9 = 22 ticks, 9,000 J/t. Eight energy upgrades on the base 1,000 J/t: `1000 * 100 / 180 = 555` J/t.

## What the bands mean

| Band | Supply that fits | Example draw |
| --- | --- | --- |
| Early, 1–5 kJ/t | One to five basic heat generators, or one advanced heat generator | One enrichment chamber is 1 kJ/t. Enrichment plus one smelter is 2 kJ/t |
| Mid, 10–50 kJ/t | Basic gas-burning on hydrogen (4 kJ/t) up through an ultimate heat generator (16 kJ/t) or a small turbine | A pipelined 5× line peaks at 15,800 J/t if every step runs at once |
| Late, 100 kJ/t and up | Ultimate ethylene gas (128 kJ/t), ultimate cable (128 kJ/t), fusion at 1,000 kJ/t | Ultimate cable fills an ultimate gas burner and does not fill a fusion reactor. An ultimate induction provider moves 1,024 kJ/t |

## One velorite ore block

Energy below uses basic machines, no upgrades, and the operation counts in the ore doc.

| Path | Ingots | Joules |
| --- | --- | --- |
| 2× | 2 | 600,000 |
| 3× | 3 | 2,560,000 |
| 4× | 4 | 5,080,000 |
| 5× | 5 | 12,440,000 |

The 5× total is 155 seconds of one basic hydrogen gas burner (4,000 J/t).

## Cables and tanks

A basic cable moves 1,000 J/t, one enrichment chamber. An ultimate cable moves 128,000 J/t, one ultimate ethylene burner. The cable buffer is that same throughput. A basic chemical tank holds 16,000 mB, the same as one interior block of a dynamic tank. Its eject rate is 250 mB/t. Fluid pipe rates, fluid tank sizes, and the per-tick split are in [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md).

## Worked-example index

```normative-examples
example.scale.enrichment=1000
example.scale.crusher=1600
example.scale.dissolution=3200
example.upgrade.speed8.ticks=22
example.upgrade.speed8.joules_per_tick=9000
example.upgrade.energy8.joules_per_tick=555
example.ore2.joules=600000
example.ore3.joules=2560000
example.ore4.joules=5080000
example.ore5.joules=12440000
example.line5.peak_joules_per_tick=15800
example.cube.basic_per_cell=1000
```
