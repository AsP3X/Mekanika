# Recipe and Data Generation

JSON shapes for the recipes whose counts are fixed in [ORE_PROCESSING_CHAIN.md](ORE_PROCESSING_CHAIN.md) and [MATERIAL_PROGRESSION.md](MATERIAL_PROGRESSION.md). Energy and tick length are not stored in the recipe. The machine reads them from config.

A data pack that replaces `mekanika:enriching/velorite_ore` changes the dust count. It does not change `machine.enrichment.joules_per_tick`.

## Normative constants

```normative
recipe.namespace=mekanika
recipe.enrich_dust_from_ore=2
recipe.infused.redstone_mb=10
recipe.base_ticks_not_in_json=200
```

## Types

| Type id | Inputs | Outputs |
| --- | --- | --- |
| `mekanika:enriching` | items | items |
| `mekanika:crushing` | items | items |
| `mekanika:smelting_machine` | items | items |
| `mekanika:purifying` | items + chemical | items |
| `mekanika:injecting` | items + chemical | items |
| `mekanika:dissolving` | items + chemical | chemical |
| `mekanika:washing` | fluid + chemical | chemical |
| `mekanika:crystallizing` | chemical | items |
| `mekanika:metallurgic_infusing` | items + chemical | items |
| `mekanika:oxidizing` | items | chemical |
| `mekanika:chemical_infusing` | two chemicals | chemical |
| `mekanika:separating` | fluid | two chemicals |
| `mekanika:crafting_shaped` | vanilla shaped | items |
| `mekanika:reacting` | 2 bio fuel + 10 mB water + 100 mB hydrogen | 100 mB ethylene + 1 substrate |

Item ingredients accept an item id or a tag. `#c:ingots/copper` is a tag. Counts are integers. Chemical and fluid amounts are millibuckets, integers.

## Enriching an ore block

`data/mekanika/recipe/enriching/velorite_ore.json`

```json
{
  "type": "mekanika:enriching",
  "input": { "item": "mekanika:velorite_ore" },
  "output": { "id": "mekanika:velorite_dust", "count": 2 }
}
```

## Infusing an alloy

`data/mekanika/recipe/metallurgic_infusing/infused_alloy.json`

```json
{
  "type": "mekanika:metallurgic_infusing",
  "item": { "tag": "c:ingots/copper" },
  "chemical": { "id": "mekanika:redstone", "amount": 10 },
  "output": { "id": "mekanika:infused_alloy", "count": 1 }
}
```

## Dissolving

`data/mekanika/recipe/dissolving/velorite_ore.json`

```json
{
  "type": "mekanika:dissolving",
  "item": { "item": "mekanika:velorite_ore" },
  "chemical": { "id": "mekanika:sulfuric_acid", "amount": 1 },
  "output": { "id": "mekanika:dirty_velorite_slurry", "amount": 1000 }
}
```

## Washing

The washer moves 5 mB of dirty slurry and 25 mB of water every tick while it has both. The recipe still records the 1:5 ratio.

```json
{
  "type": "mekanika:washing",
  "input_chemical": { "id": "mekanika:dirty_velorite_slurry", "amount": 1 },
  "input_fluid": { "tag": "c:water", "amount": 5 },
  "output": { "id": "mekanika:clean_velorite_slurry", "amount": 1 }
}
```

## Shaped circuit upgrade

`data/mekanika/recipe/crafting/advanced_control_circuit.json`

```json
{
  "type": "mekanika:crafting_shaped",
  "pattern": ["ACA"],
  "key": {
    "A": { "item": "mekanika:infused_alloy" },
    "C": { "item": "mekanika:basic_control_circuit" }
  },
  "result": { "id": "mekanika:advanced_control_circuit", "count": 1 }
}
```

## Generation

Shaped grids for blocks, upgrades, and ethylene are in [CRAFTING_RECIPES.md](CRAFTING_RECIPES.md). One data provider writes the velorite chain, the sulfur chain, the alloys, and the circuits. It reads the counts from the same constants as the normative blocks. A missing input tag skips that recipe and logs the id. Duplicate ids in a pack use the highest pack order, which is vanilla data-pack behavior.

Factories do not have their own recipe type. They look up the base machine type.

## Worked-example index

```normative-examples
example.recipe.enrich_dust=2
example.recipe.infused_redstone_mb=10
example.recipe.dissolve_dirty_mb=1000
example.recipe.wash_water_per_dirty=5
example.recipe.advanced_circuit_alloys=2
```
