# Testing Strategy

Every key in a `normative-examples` block is one assertion. The test name is the key with `example.` removed. Headless tests cover the arithmetic. Game tests cover formation, chunk unload, and the three loaders.

## Normative constants

```normative
test.loaders=3
test.headless_first=true
test.example_blocks=16
```

The 16 example blocks are the `normative-examples` fences in:

`PUBLIC_API_SUMMARY`, `NBT_CONVENTIONS`, `SYNC_STRATEGY`, `DEBUG_TOOL_SPEC`, `POWER_BUDGET`, `ORE_PROCESSING_CHAIN`, `MATERIAL_PROGRESSION`, `WORLD_GENERATION`, `HEAT_SYSTEM`, `GENERATORS_REACTORS`, `MULTIBLOCK_SPECS`, `QIO_SYSTEM`, `GEAR_MODULE_SYSTEM`, `GADGETS`, `ITEM_TRANSPORT`, `RADIATION_SYSTEM`.

`MACHINE_IO`, `SOUNDS_AND_PARTICLES`, `TEXTURE_HANDOFF_EXPANDED`, `LOCALIZATION`, `BUFFERED_NETWORKS`, `GUI_LAYOUTS`, `FACTORIES`, `CONTROL_SYSTEM`, `TIER_UPGRADING`, `UPGRADE_SYSTEM`, `JEI_EMI_INTEGRATION`, `CRAFTING_RECIPES`, `ENERGY_POWERAPI`, and this order's own index are checked by the same rule. A new example key is not done until a test asserts it.

## What each layer runs

| Layer | Asserts |
| --- | --- |
| Unit | API simulate/execute, upgrade math, ore joules, transporter costs, radiation scale, QIO caps, gear durations |
| Game | 3×3×3 tank, 5×5×5 fusion, boiler tick-5 steam, formation failure chat, chunk unload keeping controller contents |
| Manual | Large solar proxies, wind mast proxies, QIO security closing an open screen, a lined suit at 0.8 resistance |
| Loader | The unit list on Forge, Fabric, and NeoForge |

A simulation that changes stored energy fails the PowerAPI step. A 5× path that does not yield 5 ingots fails the processing step.

## Regression

A fixed bug adds one example key and one test in the same change. The test names the old wrong value in a comment only when the comment is the assertion input. Player-visible text is compared to [FAILURE_MODES.md](FAILURE_MODES.md) and to the message tables already in the system docs.

## Worked-example index

```normative-examples
example.layers=4
example.loaders=3
example.ore5.ingots=5
example.ore5.joules=12440000
```
