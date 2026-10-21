# Generators and Reactors

Early generators produce Joules from fuel or the environment. The heat generator also fills a heat buffer. Conductor math is in [HEAT_SYSTEM.md](HEAT_SYSTEM.md). Turbine, fission, and fusion ticks stay in [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md).

Joules are the energy unit from [ENERGY_POWERAPI.md](ENERGY_POWERAPI.md). Rates below are basic-tier defaults. Advanced, Elite, and Ultimate multiply those rates by 3, 8, and 16.

## Normative constants

```normative
generator.tier_mult.basic=1
generator.tier_mult.advanced=3
generator.tier_mult.elite=8
generator.tier_mult.ultimate=16
generator.buffer_ticks=20

generator.heat.energy_per_tick=1000
generator.heat.heat_per_tick=40
generator.heat.coal_ticks=1600
generator.heat.lava_heat_per_face=10
generator.heat.lava_energy_per_face=200
generator.heat.capacity=1000

generator.solar.energy_per_tick=250
generator.solar.rain_divisor=2

generator.wind.energy_per_tick=200
generator.wind.min_y=64
generator.wind.max_y=189
generator.wind.permille_per_y=8
generator.wind.rain_num=3
generator.wind.rain_den=2

generator.bio.energy_per_tick=800
generator.bio.ticks_per_hunger=40
generator.bio.min_ticks=40

generator.gas.tank_mb=32000
generator.gas.mb_per_tick=1
generator.gas.hydrogen_j_per_mb=4000
generator.gas.ethylene_j_per_mb=8000
```

`generator.heat.output` in [CONFIG_STRUCTURE.md](CONFIG_STRUCTURE.md) is `generator.heat.energy_per_tick`.

Tier installers preserve energy, heat, fuel, chemical contents, side configuration, and redstone mode.

## Shared generator rules

Each generator is one block and one block entity. The energy buffer holds `tierRate * generator.buffer_ticks` Joules. When the buffer cannot take a full tick of production, a fueled generator skips that tick and does not consume fuel. Solar and wind have no fuel. Energy they cannot store is discarded.

Energy side modes are Output and None. The default is Output on every face. Redstone uses the modes in [CONTROL_SYSTEM.md](CONTROL_SYSTEM.md). A redstone-off machine produces nothing and does not consume fuel.

The comparator reads fuel progress while a fuel burn is active: `floor(remainingTicks * 15 / burnTicks)`. Solar and wind read `floor(permille * 15 / 1000)`. An idle fueled generator reads the energy buffer fill.

Generators tick only while their chunk is loaded.

Status values are `idle`, `running`, `output_full`, `redstone_off`, and the extra values in each chapter.

| Id suffix | English |
| --- | --- |
| `heat_generator_basic` | Basic Heat Generator |
| `heat_generator_advanced` | Advanced Heat Generator |
| `heat_generator_elite` | Elite Heat Generator |
| `heat_generator_ultimate` | Ultimate Heat Generator |
| `solar_generator_basic` | Basic Solar Generator |
| `solar_generator_advanced` | Advanced Solar Generator |
| `solar_generator_elite` | Elite Solar Generator |
| `solar_generator_ultimate` | Ultimate Solar Generator |
| `wind_generator_basic` | Basic Wind Generator |
| `wind_generator_advanced` | Advanced Wind Generator |
| `wind_generator_elite` | Elite Wind Generator |
| `wind_generator_ultimate` | Ultimate Wind Generator |
| `bio_generator_basic` | Basic Bio Generator |
| `bio_generator_advanced` | Advanced Bio Generator |
| `bio_generator_elite` | Elite Bio Generator |
| `bio_generator_ultimate` | Ultimate Bio Generator |
| `gas_burning_generator_basic` | Basic Gas-Burning Generator |
| `gas_burning_generator_advanced` | Advanced Gas-Burning Generator |
| `gas_burning_generator_elite` | Elite Gas-Burning Generator |
| `gas_burning_generator_ultimate` | Ultimate Gas-Burning Generator |
| `bounding_proxy` | Bounding Proxy |

All of those ids use the `mekanika` namespace.

## Heat Generator

Burns an item and, while that burn is active, adds both Joules and heat points.

Basic fueled rates are 1,000 J/t and 40 heat points per tick. The tier multiplier applies to both. Heat is added to a buffer of `1000 * multiplier` points. The generator's conduction rate equals its heat-per-tick value. Coupling rules are in the heat document.

### Fuels

Files under `data/mekanika/heat_fuel/` replace the defaults per item. The default coal entry is `data/mekanika/heat_fuel/coal.json`:

```json
{
  "item": "#minecraft:coals",
  "ticks": 1600,
  "energy_per_tick": 1000,
  "heat_per_tick": 40
}
```

Those two rates are basic-tier rates. The block multiplies them. One coal in a basic generator therefore yields 1,600,000 J and 64,000 heat points over 1,600 ticks, if the energy buffer and heat consumers keep up. A skipped tick because the energy buffer is full does not advance the burn.

The GUI has one fuel slot, an energy bar, a heat bar, and the burn remaining.

### Lava

Each face touching a lava source adds heat and energy even when no item is burning. Flowing lava does not count.

`lavaHeat = min(heatPerTick, lavaFaces * 10 * multiplier)`

`lavaEnergy = min(energyPerTick / 2, lavaFaces * 200 * multiplier)`

`heatPerTick` and `energyPerTick` are the tier rates. Lava energy uses integer division.

Basic, one lava face: 10 heat and 200 J per tick. Basic, six lava faces: 40 heat and 500 J per tick. Lava output still respects a full energy buffer by dropping only the Joule part. The heat part is still added, then capped by the heat buffer.

Status `no_fuel` means there is no active burn and no lava faces.

## Solar Generator

No fuel. Full-sun basic output is 250 J/t before the tier multiplier.

The sky test matches the evaporation plant:

- The column above the block is air, up to build height.
- The dimension has skylight.
- `dayTime % 24000 < 12000`.
- It is not thundering.

Failing any of those yields 0. Rain divides the result by 2 after the multiplier.

Clear sun produces `tierBase` Joules per tick. Rain produces `tierBase / 2`. `tierBase = 250 * multiplier`.

A basic generator in clear sun for 100 ticks stores 25,000 J. The same generator in rain produces 125 J/t.

Basic and Advanced panels sit inside the placed block. Their sky test is that single column.

Elite and Ultimate are large panels. The logic block is one anchor. It also claims the cell at `y + 1` and the 3×3 at `y + 2`, which is the center plus the eight neighbors. Placement fails unless every one of those cells is air. On placement they become proxies, as defined in Rendering. If a proxy later goes missing, status is `no_sun` and output is 0.

Status `no_sun` means the sky test failed or the span is broken.

## Wind Generator

No fuel. Basic output at full wind is 200 J/t before the tier multiplier.

`altitude = clamp(y, 64, 189)`

`permille = (altitude - 64) * 8`

Rain replaces `permille` with `min(1000, permille * 3 / 2)`. Thunder sets `permille` to 1000. A dimension without weather keeps the altitude term only.

`output = tierBase * permille / 1000` with `tierBase = 200 * multiplier`.

| Placement | Basic output |
| --- | --- |
| y = 64, clear | 0 |
| y = 114, clear | 80 J/t |
| y = 114, rain | 120 J/t |
| y = 189, clear | 200 J/t |

The mast claims the four blocks above the anchor. Placement fails unless those four are air. On placement they become proxies. If a proxy later goes missing, status is `blocked` and output is 0.

Status `calm` means permille is 0. Status `blocked` means the mast span is broken.

## Bio Generator

One food slot. Hunger comes from the item's food component. `burnTicks = max(40, hunger * 40)`. Bread is 5 hunger, so 200 ticks.

Basic rate is 800 J/t times the tier multiplier. Bread in a basic generator yields 160,000 J over 200 ticks when the buffer accepts every tick.

A datapack file `data/mekanika/bio_fuel/<id>.json` can set `ticks` and `energy_per_tick` for a non-food item. Those rates are basic-tier rates. The file wins over hunger when both match.

The generator does not produce conductor heat.

Status `no_fuel` when the slot is empty and no burn is active.

## Gas-Burning Generator

Burns one chemical at a time from a 32,000 mB tank. Consumption is 1 mB per tick at every tier. The tier multiplier scales Joules only, so a higher tier gets more energy from the same gas.

| Chemical | Basic J per mB | Ultimate J/t |
| --- | --- | --- |
| `mekanika:hydrogen` | 4,000 | 64,000 |
| `mekanika:ethylene` | 8,000 | 128,000 |

Hydrogen is the electrolysis output named in [CHEMICAL_SYSTEM.md](CHEMICAL_SYSTEM.md). Ethylene is the reaction output in [CRAFTING_RECIPES.md](CRAFTING_RECIPES.md): 100 mB from 2 bio fuel, 10 mB of water, and 100 mB of hydrogen. Both are gases. Neither is radioactive.

Files under `data/mekanika/gas_fuel/` can add another chemical. Each file sets `chemical` and `joules_per_mb` at the basic tier.

The GUI shows the chemical tank, the burn rate, and the energy bar. Side modes for the chemical are Input and None. The default is Input on every face. Energy stays Output.

Status `no_fuel` when the tank does not hold 1 mB of a known fuel.

## Rendering

The large solar panel in Mekanism's public generators code is one logic block, not a formed multiblock. The block model is a cutout JSON model whose elements extend past the 0–16 cube: a base, a mast, and a panel about three blocks wide and three blocks deep. The collision shape uses those same boxes, moved up one block so the base sits in the placed cell, then rotated for each horizontal facing. The cells the panel and mast occupy are not left empty. A bounding handler claims the block above the anchor and the full 3×3 two blocks above it. The wind generator claims the four blocks directly above its anchor and uses a tall selection shape. Heat, bio, and the small solar generator stay inside one block.

Mekanika uses that split and does not reuse the meshes, textures, or the one-block-low model offset. Claude draws original models whose base already sits in the anchor.

| Generator | What is placed |
| --- | --- |
| Heat, bio, gas, basic solar, advanced solar | One block. Model and collision stay inside it |
| Elite and ultimate solar | Anchor, plus proxy at `y + 1`, plus proxies on the 3×3 at `y + 2`. Model: 4-pixel mast from 4 px to 48 px, and a 48×48×2 px panel centered on the anchor with its top at 48 px |
| Wind, every tier | Anchor, plus four proxies at `y + 1` through `y + 4`. Model: a 6-pixel mast from 0 px to 80 px |

Proxy rules:

- The proxy block id is `mekanika:bounding_proxy`. It stores the anchor position. The full list of blocks that use it is in [OVERSIZED_BLOCKS.md](OVERSIZED_BLOCKS.md).
- Proxies are invisible. They have no outline and no collision of their own.
- The anchor's collision shape covers the mast and, for a large panel, the wings. It rotates with the anchor's horizontal facing.
- Using or configuring a proxy opens the anchor.
- Breaking the anchor drops the generator once and removes every proxy.
- Breaking a proxy does the same.
- The anchor does not occlude. Its render layer is cutout.
- Item and GUI displays scale the model down until the panel or mast fits.

Coordinates are pixels in block-model space. 0–16 is the anchor. 16 is one block up.

## Where the later sources sit

These are the structure-spec rates, copied here so the early tables have a ceiling. The tick rules are not repeated.

| Source | Output |
| --- | --- |
| Small turbine, 5×5×5, the worked blade-limited case | 24,000 J/t at 100.0°C steam |
| Tall turbine, 5×5×17, the worked vent-limited case | 36,000 J/t at 100.0°C steam |
| Running fusion reactor | 1,000,000 J/t |

Fission does not push Joules. Its GUI field `steamEnergyJoules` uses the turbine's joules-per-mB formula.

## Multiblock pointers

### Industrial Turbine

Steam enters as `mekanika:steam`. Energy leaves through the controller. Blades, coils, vents, and the limiting-status order are in the structure spec.

### Fission Reactor

Consumes `mekanika:fissile_fuel` and water. Produces steam and `mekanika:spent_fuel`. Uncooled heat scrams on tick 200 for one assembly. Conductor networks do not attach.

### Fusion Reactor

Fixed 5×5×5 shell, four lasers, one fuel port, one core. Ignition takes 10,000,000 J. The first electrical output tick of the worked start is tick 301. A dry fuel run of 20 ticks quenches.

## Debug fields

- Tier and status
- Energy buffer and this tick's Joule output
- Heat buffer, for the heat generator
- Fuel id, remaining ticks, or chemical amount
- Sun or wind permille, when those apply
- Lava face count, for the heat generator

## Worked-example index

```normative-examples
example.heatgen.coal_energy_j=1600000
example.heatgen.coal_heat=64000
example.heatgen.one_lava.energy=200
example.heatgen.one_lava.heat=10
example.heatgen.six_lava.energy=500
example.heatgen.six_lava.heat=40
example.solar.basic_clear_100_ticks=25000
example.solar.basic_rain_j_per_tick=125
example.wind.y64=0
example.wind.y114=80
example.wind.y114_rain=120
example.wind.y189=200
example.bio.bread_j=160000
example.gas.hydrogen_basic=4000
example.gas.hydrogen_ultimate=64000
example.gas.ethylene_ultimate=128000
```
