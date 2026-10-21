# JEI and EMI

Recipe lookup for the twelve machine types, plus one info page per multiblock. JEI and EMI are optional. Mekanika runs when neither is installed. Both plugins register the same categories. A data pack that replaces a recipe changes the page. It does not change the joules in config.

The layout uses the machine presets in [GUI_LAYOUTS.md](GUI_LAYOUTS.md). The counts come from the recipe JSON in [RECIPE_DATA_GENERATION.md](RECIPE_DATA_GENERATION.md).

## Normative constants

```normative
jei.machine_categories=12
jei.structure_pages=8
jei.catalysts_per_category=5
jei.required_mods=0
jei.ghost.saves=0
```

`12 + 8 = 20` pages. `12 * 5 = 60` catalyst registrations. Vanilla `mekanika:crafting_shaped` stays in the crafting grid. It is not one of the twelve.

## Categories

The category id is the recipe type id.

| Category | Catalysts |
| --- | --- |
| `mekanika:enriching` | Enrichment chamber, then its four factories |
| `mekanika:crushing` | Crusher and its four factories |
| `mekanika:smelting_machine` | Energized smelter and its four factories |
| `mekanika:purifying` | Purification chamber and its four factories |
| `mekanika:injecting` | Chemical injection chamber and its four factories |
| `mekanika:dissolving` | Chemical dissolution chamber and its four factories |
| `mekanika:washing` | Chemical washer and its four factories |
| `mekanika:crystallizing` | Chemical crystallizer and its four factories |
| `mekanika:metallurgic_infusing` | Metallurgic infuser and its four factories |
| `mekanika:oxidizing` | Chemical oxidizer and its four factories |
| `mekanika:chemical_infusing` | Chemical infuser and its four factories |
| `mekanika:separating` | Electrolytic separator and its four factories |

The page is the machine preset from the layout doc, without the player inventory. Item slots, gauges, and the progress widget sit at those coordinates. The energy line reads the config joules per tick. A batch machine also shows `joules_per_tick * 200`. A flow machine shows the per-tick rate and does not show a batch total.

Enriching velorite ore shows 1 ore, 2 dust, and 1,000 J/t. The batch line is 200,000 J. Infusing an alloy shows 10 mB of `mekanika:redstone`. The washer page shows 1,000 J/t and no batch total.

The tooltip adds one static line: eight speed upgrades take the batch to 22 ticks at 9,000 J/t. It does not recompute other upgrade counts.

## Transfer

Clicking a recipe while a machine or factory screen of that category is open transfers items from the player inventory into input slots.

- The amount moved is the minimum of the recipe count, the player's count, and the free space in the slot.
- A factory uses the lowest index slot that can take the item.
- A tag ingredient uses the first matching item the player is holding. If the player has none, the ghost shows the first item currently in that tag.
- A missing item places a ghost in the slot. A ghost is not processed, is not saved, and clears when the screen closes.
- Chemical and fluid amounts are drawn on the gauge. They move 0 mB.
- The filter upgrade still rejects a different item id. That transfer moves 0 and does not place a ghost.
- A screen of another category moves 0.

The player holds 5 velorite ore and the enrichment input is empty. The transfer moves 1 and leaves 4. The player holds 0. The transfer moves 0 and the slot shows a ghost.

## Structure pages

These ids are info pages. They have no inputs and no transfer.

| Page | Minimum |
| --- | --- |
| `mekanika:structure/dynamic_tank` | Exterior 3. Shell `27 - 1 = 26` blocks |
| `mekanika:structure/evaporation` | Footprint 4×4, height 3 |
| `mekanika:structure/boiler` | Exterior 3 |
| `mekanika:structure/sps` | Exterior 3, 5, or 7 |
| `mekanika:structure/induction_matrix` | Exterior 3, interior filled |
| `mekanika:structure/turbine` | Exterior 5, odd width |
| `mekanika:structure/fission` | Exterior 3 |
| `mekanika:structure/fusion` | Exterior 5. Frame `125 - 27 = 98` blocks |

Each page names the controller id and the minimum exterior. The tank page states the interior block is air and the minimum shell is 1 controller plus 25 casings. The fusion page states the frame is 1 controller, 1 fuel port, 4 lasers, and 92 casings, plus the core. There is no 3D preview.

## Worked-example index

```normative-examples
example.categories=12
example.structures=8
example.pages=20
example.catalysts=5
example.catalyst_registrations=60
example.required_mods=0
example.energy.per_tick=1000
example.energy.batch=200000
example.washer.batch_line=0
example.transfer.moved=1
example.transfer.left=4
example.ghost.moved=0
example.chemical.moved=0
example.tank.shell=26
example.fusion.frame=98
example.fusion.casings=92
```
