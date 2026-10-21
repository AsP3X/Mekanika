# World Generation

Overworld placement for velorite and sulfur. Recipe use of those items is in [ORE_PROCESSING_CHAIN.md](ORE_PROCESSING_CHAIN.md) and [MATERIAL_PROGRESSION.md](MATERIAL_PROGRESSION.md).

Existing chunks do not change. New chunks use the current config. A data pack can replace the placed feature. The config keys disable a feature. They do not rewrite a feature that a pack already replaced.

## Normative constants

```normative
worldgen.velorite.enabled=true
worldgen.velorite.vein_size=8
worldgen.velorite.veins_per_chunk=6
worldgen.velorite.min_y=-32
worldgen.velorite.max_y=48
worldgen.velorite.distribution=uniform

worldgen.sulfur.enabled=true
worldgen.sulfur.vein_size=6
worldgen.sulfur.veins_per_chunk=4
worldgen.sulfur.min_y=0
worldgen.sulfur.max_y=96
worldgen.sulfur.distribution=uniform

worldgen.salt.enabled=false
worldgen.salt.vein_size=6
worldgen.salt.veins_per_chunk=3
worldgen.salt.min_y=40
worldgen.salt.max_y=80

worldgen.fortune.extra_per_level=1
worldgen.fortune.max_drops=4
```

Uniform means each Y in the closed range is equally likely. Veins do not generate in a chunk when the key is false. Salt is off until a recipe needs it. Sulfur is on because sulfuric acid starts from sulfur dust.

## Drops

| Block | Silk Touch | Otherwise |
| --- | --- | --- |
| `mekanika:velorite_ore` | The ore block | 1 raw velorite, plus 1 per Fortune level, capped at 4 |
| `mekanika:deepslate_velorite_ore` | The deepslate ore | Same raw count |
| `mekanika:sulfur_ore` | The ore block | 2 to 4 sulfur dust, plus 1 per Fortune level, capped at 6 |

The 2×–5× machine counts are for the ore block item. Raw velorite uses the raw table in the ore doc. Furnace smelting of one raw velorite or one ore block yields one ingot.

No biome filter is applied. A config list `worldgen.<ore>.biomes` may name biome ids. An empty list means every overworld biome. The Nether and the End do not place these ores.

## Tags

| Tag | Members |
| --- | --- |
| `c:ores/velorite` | Both velorite ore blocks |
| `c:ores/osmium_style` | Both velorite ore blocks |
| `c:raw_materials/velorite` | Raw velorite |
| `c:ingots/velorite` | Velorite ingot |
| `c:dusts/sulfur` | Sulfur dust |

## Worked-example index

```normative-examples
example.velorite.band=80
example.velorite.fortune3_drops=4
example.sulfur.band=96
example.salt.enabled=false
```
