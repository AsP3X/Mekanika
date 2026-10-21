# Ore Processing Chain (2× to 5×)

Counts for one ore block of the primary metal, velorite. Machine energy and the 200-tick batch length are in [POWER_BUDGET.md](POWER_BUDGET.md). The public Mekanism ore recipes are the source of the item and millibucket counts. The step order below is that public order.

Raw ore is a separate table. It does not reach five ingots per raw item.

## Normative constants

```normative
ore.batch_ticks=200
ore.enrich_dust_from_ore=2
ore.purify_clumps_from_ore=3
ore.purify_oxygen_mb=1
ore.inject_shards_from_ore=4
ore.inject_hcl_mb=1
ore.dissolve_sulfuric_mb=1
ore.dissolve_dirty_mb=1000
ore.wash_dirty_mb_per_tick=5
ore.wash_water_mb_per_tick=25
ore.crystal_clean_mb=200
ore.crystals_from_ore=5
ore.raw_in_for_four_dust=3
ore.dust_from_three_raw=4
ore.clumps_from_one_raw=2
ore.shards_from_three_raw=8
ore.dirty_mb_from_three_raw=2000
```

## Machines

| Machine | Role | J/t | Rhythm |
| --- | --- | --- | --- |
| Enrichment chamber | Ore or dirty dust to dust | 1,000 | 200 ticks per batch |
| Crusher | Clump to dirty dust, ingot to dust | 1,600 | 200 ticks |
| Energized smelter | Dust to ingot | 1,000 | 200 ticks |
| Purification chamber | Item + oxygen to the previous solid | 2,000 | 200 ticks |
| Chemical injection chamber | Item + hydrogen chloride to the previous solid | 3,000 | 200 ticks |
| Chemical dissolution chamber | Ore + sulfuric acid to dirty slurry | 3,200 | 200 ticks |
| Chemical washer | Dirty slurry + water to clean slurry | 1,000 | 5 mB dirty and 25 mB water every tick |
| Chemical crystallizer | 200 mB clean slurry to one crystal | 3,000 | 200 ticks |

One dust smelts to one ingot. One clump crushes to one dirty dust. One dirty dust enriches to one dust. One shard purifies to one clump. One crystal injects to one shard.

## Ore block paths

Smelting the ore block in a furnace gives 1 ingot. The machines below replace that.

### 2×

One enrichment batch turns 1 ore block into 2 dust. Two smelter batches turn those into 2 ingots. No chemicals. Energy 600,000 J.

### 3×

One purification batch: 1 ore block + 1 mB oxygen → 3 clumps. Each clump is crushed, enriched, and smelted. Energy 2,560,000 J. Oxygen used: 1 mB.

### 4×

One injection batch: 1 ore block + 1 mB hydrogen chloride → 4 shards. Each shard takes 1 mB oxygen in a purification batch, then the 3× tail. Energy 5,080,000 J. Chemicals: 1 mB hydrogen chloride and 4 mB oxygen.

### 5×

1. Dissolution, one batch: 1 ore block + 1 mB sulfuric acid → 1,000 mB dirty slurry.
2. Washer, 200 ticks: 5 mB dirty and 25 mB water each tick. That is 1,000 mB dirty and 5,000 mB water in, and 1,000 mB clean slurry out.
3. Crystallizer, five batches: 200 mB clean slurry → 1 crystal. Five crystals.
4. Each crystal: 1 mB hydrogen chloride → 1 shard → 1 mB oxygen → 1 clump → dirty dust → dust → ingot.

Energy 12,440,000 J. Chemicals for the whole ore: 1 mB sulfuric acid, 5,000 mB water, 5 mB hydrogen chloride, 5 mB oxygen. Five ingots.

If every 5× machine runs on the same tick, the draw is 15,800 J/t. That sum is dissolution 3,200 + washer 1,000 + crystallizer 3,000 + injection 3,000 + purification 2,000 + crusher 1,600 + enrichment 1,000 + smelter 1,000.

## Raw velorite

| Input | Machine | Output |
| --- | --- | --- |
| 3 raw | Enrichment, one batch | 4 dust |
| 1 raw + 1 mB oxygen | Purification | 2 clumps |
| 3 raw + 1 mB hydrogen chloride | Injection | 8 shards |
| 3 raw + 1 mB sulfuric acid | Dissolution | 2,000 mB dirty slurry |

Furnace smelting of 1 raw velorite gives 1 ingot.

## Chemical supply

These recipes keep a 3× or 4× line fed. Amounts are millibuckets.

| Recipe | Inputs | Outputs | Machine |
| --- | --- | --- | --- |
| Electrolysis | 2 water each tick | 1 hydrogen + 1 oxygen | Electrolytic separator, 1,000 J/t |
| Brine split | 2 brine each tick | 1 hydrogen + 1 chlorine | Same separator, brine mode |
| Hydrogen chloride | 1 hydrogen + 1 chlorine each tick | 1 hydrogen chloride | Chemical infuser, 3,000 J/t |
| Sulfur dioxide | 1 sulfur dust | 100 sulfur dioxide in one 200-tick oxidizer batch | Oxidizer, 3,000 J/t |
| Sulfur trioxide | 1 sulfur dioxide + 1 oxygen each tick | 1 sulfur trioxide | Chemical infuser |
| Sulfuric acid | 1 sulfur trioxide + 1 water each tick | 1 sulfuric acid | Chemical infuser |

Water in the sulfuric step is the fluid `minecraft:water`. The separator's water is the same fluid.

Ids: `mekanika:oxygen`, `mekanika:hydrogen`, `mekanika:chlorine`, `mekanika:hydrogen_chloride`, `mekanika:sulfur_dioxide`, `mekanika:sulfur_trioxide`, `mekanika:sulfuric_acid`, `mekanika:dirty_velorite_slurry`, `mekanika:clean_velorite_slurry`.

## Worked-example index

```normative-examples
example.ore2.ingots=2
example.ore2.joules=600000
example.ore3.ingots=3
example.ore3.oxygen_mb=1
example.ore3.joules=2560000
example.ore4.ingots=4
example.ore4.hcl_mb=1
example.ore4.oxygen_mb=4
example.ore4.joules=5080000
example.ore5.ingots=5
example.ore5.sulfuric_mb=1
example.ore5.water_mb=5000
example.ore5.hcl_mb=5
example.ore5.oxygen_mb=5
example.ore5.joules=12440000
example.ore5.peak_joules_per_tick=15800
example.wash.ticks_per_ore=200
example.crystal.ops_per_ore=5
```
