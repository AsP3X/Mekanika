# Machine I/O

Slot counts and default sides for every processing block. Joules per tick stay in [POWER_BUDGET.md](POWER_BUDGET.md). A factory uses the same tanks and the same sides as its base machine. Its item slots repeat once per factory slot. Factory slot counts are `factory.slots.*` in the power budget.

## Normative constants

```normative
machine.upgrade_slots=8
machine.charge_pad.joules_per_tick=1000
machine.enrichment.item_in=1
machine.enrichment.item_out=1
machine.washer.item_slots=0
machine.separator.chemical_out=2
machine.infuser.chemical_in=1
machine.chemical_infuser.chemical_in=2
```

## Shared layout

Input item slots are on the left of the GUI. Output item slots are on the right. The progress arrow sits between them. The energy bar is the left edge. Chemical and fluid gauges are vertical bars beside the slots they serve. Eight upgrade slots sit in a column on the right. The redstone, side, and security buttons sit on the top row. Pixel positions are in [GUI_LAYOUTS.md](GUI_LAYOUTS.md).

Default sides, before the player changes them:

| Side | Items | Fluids | Chemicals | Energy |
| --- | --- | --- | --- | --- |
| Back | Input | None | None | Input |
| Front | Output | None | None | None |
| Left | None | None | Input | Input |
| Right | None | None | Output | Input |
| Top | None | Input | None | Input |
| Bottom | None | None | None | Input |

A machine that has no slot of that kind leaves the mode at none. Energy input is every side except the front.

## Processing machines

| Block id | Item in | Item out | Fluid in | Chemical in | Chemical out | J/t |
| --- | --- | --- | --- | --- | --- | --- |
| `enrichment_chamber` | 1 | 1 | 0 | 0 | 0 | 1,000 |
| `crusher` | 1 | 1 | 0 | 0 | 0 | 1,600 |
| `energized_smelter` | 1 | 1 | 0 | 0 | 0 | 1,000 |
| `purification_chamber` | 1 | 1 | 0 | 1 | 0 | 2,000 |
| `chemical_injection_chamber` | 1 | 1 | 0 | 1 | 0 | 3,000 |
| `chemical_dissolution_chamber` | 1 | 0 | 0 | 1 | 1 | 3,200 |
| `chemical_washer` | 0 | 0 | 1 | 1 | 1 | 1,000 |
| `chemical_crystallizer` | 0 | 1 | 0 | 1 | 0 | 3,000 |
| `metallurgic_infuser` | 1 | 1 | 0 | 1 | 0 | 1,000 |
| `chemical_oxidizer` | 1 | 0 | 0 | 0 | 1 | 3,000 |
| `chemical_infuser` | 0 | 0 | 0 | 2 | 1 | 3,000 |
| `electrolytic_separator` | 0 | 0 | 1 | 0 | 2 | 1,000 |
| `modification_station` | 2 | 1 | 0 | 0 | 0 | 1,000 |

The modification station's two inputs are the gear piece and the module item. The output is the gear piece after the 200-tick install. Its energy matches `gear.station.joules_per_tick`. The block also claims three cells outside itself: above, to the right of its facing, and above that right cell. Those cells are `bounding_proxy` blocks. The pattern is in [OVERSIZED_BLOCKS.md](OVERSIZED_BLOCKS.md).

Factory ids are `<machine>_factory_<tier>` for each of the twelve processing machines and each of basic, advanced, elite, and ultimate. That is 48 factory blocks. The station has no factory.

`pressurized_reaction_chamber` is not one of the twelve. It has 1 item input, 1 item output, 1 fluid input, 1 chemical input, and 1 chemical output. It draws 3,000 J/t. The recipe is in [CRAFTING_RECIPES.md](CRAFTING_RECIPES.md).

## Storage and charge

| Block | Slots | Buffer | Notes |
| --- | --- | --- | --- |
| `energy_cube_<tier>` | 0 | The cube capacity in the power budget | Energy input and output, default both on every face |
| `chemical_tank_<tier>` | 1 fill, 1 drain | The tank capacity in the power budget | One chemical. Faces and the per-tick rate are in [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md) |
| `fluid_tank_<tier>` | 1 fill, 1 drain | 32,000 / 64,000 / 128,000 / 256,000 mB | One fluid. Bottom is output. A bucket moves 1,000 mB |
| `charge_pad` | 1 charge | 20,000 J | Moves 1,000 J/t into the item. Energy input on every face except the top |
| `waste_barrel` | 0 | 512,000 mB of spent fuel | No GUI slots. Use inserts spent fuel from the face |

Tier suffixes are `basic`, `advanced`, `elite`, and `ultimate`.

## Generators

Heat generators have one fuel slot. Bio generators have one food slot. Gas-burning generators have one chemical tank of 32,000 mB and no item slots. Solar and wind generators have no item slots and no tanks. Energy leaves through faces set to output. The default is output on every face, from [GENERATORS_REACTORS.md](GENERATORS_REACTORS.md).

## Worked-example index

```normative-examples
example.upgrade_slots=8
example.enrichment.item_slots=2
example.washer.item_slots=0
example.separator.chemical_out=2
example.factories=48
example.charge_pad.buffer=20000
```
