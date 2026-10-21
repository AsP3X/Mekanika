# Blocks That Draw Outside Their Cube

Mekanism's current generators and machines code (commit `d4df9b35`) has two ways to draw past one block. This file lists every one of those blocks and the Mekanika choice for each.

The shared proxy is `mekanika:bounding_proxy`. It stores the anchor position. It is invisible, has no collision, and has no outline. Using it or breaking it acts on the anchor. Breaking the anchor removes every proxy and drops the anchor once.

## Normative constants

```normative
oversized.proxy=mekanika:bounding_proxy
oversized.solar.cells=10
oversized.wind.cells=4
oversized.miner.cells=17
oversized.station.cells=3
oversized.visual_only=5
```

Ten solar cells are the mast at `y + 1` plus the nine cells of the 3×3 at `y + 2`. Seventeen miner cells are a 3×2×3 box minus the anchor. Three station cells are above the anchor, to the right of its facing, and above that right cell. Four wind cells are `y + 1` through `y + 4`.

## Reserved space

These Mekanism blocks place bounding blocks. The model is drawn from the anchor. The extra cells only keep other blocks out and forward clicks.

| Mekanism block | Cells claimed | Mekanika block |
| --- | --- | --- |
| Advanced solar generator | Above the anchor, and the 3×3 two blocks above | `solar_generator_elite` and `solar_generator_ultimate`. The panel is 48×48 pixels with its top at 48 px. See [GENERATORS_REACTORS.md](GENERATORS_REACTORS.md) |
| Wind generator | Four blocks above the anchor | Every `wind_generator_<tier>`. The blade mesh is drawn about 4.5 blocks above the anchor |
| Digital miner | The other 17 cells of a 3-wide, 2-tall, 3-deep box | Not a scheduled block. A future miner uses this same box |
| Modification station | Above, to the right of the facing, and above that right cell | `modification_station` |

The advanced solar model is stored one block low, with a JSON translation of `[0, 1, 0]`, because vanilla model coordinates stop at 32. The collision boxes are moved up one block to match. Mekanika authors the base in the anchor and does not use that offset. The wind blades are a second model drawn by the block entity, offset by half a block in X and Z and 4.5 blocks up. The mast collision reaches about 75.5 pixels, which still fits in the four cells above the anchor.

`modification_station` fails to place unless those three cells are air. Its GUI slots stay the two inputs and one output in [MACHINE_IO.md](MACHINE_IO.md). The extra width is only the model and the proxies.

## Visual overflow only

These Mekanism models leave the cube and do not claim a second cell. Another block can be placed where the mesh sticks out.

| Mekanism block | Overflow | Mekanika |
| --- | --- | --- |
| Isotopic centrifuge | Straight up to 32 px, a second block tall, footprint still 1×1 | Not scheduled. If added, claim the cell above |
| Pigment mixer | Straight up to 32 px | Not scheduled. If added, claim the cell above |
| Seismic vibrator | Up to 32 px, and about 10 px past the sides | Not scheduled. If added, claim the cell above |
| Solar neutron activator | Up to about 29 px, and about 1 px past the sides | Not scheduled. If added, claim the cell above |
| Security desk | Up to 24 px | `security_desk` stays inside one block |

Smaller overflows are not copied. The gas-burning generator mesh reaches about 18 px. The logistical sorter reaches about 17 px. The fluidic plenisher shape dips about 7 px into the block below. Our gas generator, sorter, and pipes stay inside the placed block.

## Worked-example index

```normative-examples
example.solar.cells=10
example.wind.cells=4
example.miner.cells=17
example.station.cells=3
example.visual_only=5
example.security_desk.proxies=0
```
