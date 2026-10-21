# Gear and Module System

MekaSuit and Meka-Tool modules. Energy is the joule from [ENERGY_POWERAPI.md](ENERGY_POWERAPI.md). Radiation resistance caps at 1, and the lined suit's 0.8 is defined in [RADIATION_SYSTEM.md](RADIATION_SYSTEM.md).

Mekanism's public module map is the slot split used here: helmet, chest, legs, boots, and the held tool each accept their own set. Exclusive flight modules and the silk/fortune pair follow that split. The joule costs are Mekanika's, on the same scale as a 1,000 J/t enrichment chamber.

## Normative constants

```normative
gear.module_slots=8
gear.suit.base_energy=1000000
gear.tool.base_energy=500000
gear.energy_unit.capacity=1000000
gear.energy_unit.max=8
gear.jetpack.joules_per_tick=50
gear.jetpack.hydrogen_mb_per_tick=1
gear.jetpack.tank_mb=32000
gear.grav.joules_per_tick=200
gear.radiation.level1=0.5
gear.radiation.level2=1.0
gear.radiation.max=2
gear.charge.joules_per_tick=1000
gear.vision.joules_per_tick=10
gear.speed.bonus_per_level=0.2
gear.speed.max=4
gear.speed.joules_per_tick=20
gear.jump.max=3
gear.jump.joules=30
gear.fall.joules=40
gear.magnet.radius=4
gear.magnet.joules_per_item=5
gear.tool.joules_per_block=100
gear.tool.vein_blocks_per_level=16
gear.tool.vein.max=3
gear.tool.fortune.max=3
gear.tool.efficiency.max=4
gear.tool.attack.bonus_per_level=2
gear.tool.attack.max=4
gear.tool.attack.joules=100
gear.tool.use.joules=20
gear.station.joules_per_tick=1000
gear.station.ticks=200
```

## Pieces

| Piece | Base energy | Accepts |
| --- | --- | --- |
| `mekanika:mekasuit_helmet` | Draws from the chest | Vision, breathing, nutrition |
| `mekanika:mekasuit_chest` | 1,000,000 J, plus energy modules | Energy, jetpack, grav, radiation, charge, dosimeter |
| `mekanika:mekasuit_legs` | Draws from the chest | Speed, jump |
| `mekanika:mekasuit_boots` | Draws from the chest | Fall, magnet |
| `mekanika:meka_tool` | 500,000 J of its own | Silk, fortune, efficiency, vein, attack, farming, shears |

Each piece holds at most 8 module types. A second copy of an installed module raises its level by 1, up to that module's max, and still uses one type slot. The MekaSuit occupies the four armor slots. It does not combine with the lined suit or with the jetpack and free-runner gadgets. Those gadgets work only in an empty armor slot.

The chest is the only suit battery. Legs, boots, and the helmet spend that battery. The tool spends its own battery. The charge module, while enabled, moves up to 1,000 J/t from the chest into the tool and stops when the tool is full.

## Modules

Disabled modules stay installed, draw nothing, and do not conflict. Enabling a module that conflicts with an enabled module is refused.

| Id | Piece | Max | Draw | Effect |
| --- | --- | --- | --- | --- |
| `vision_unit` | Helmet | 1 | 10 J/t while enabled | Night vision |
| `breathing_unit` | Helmet | 1 | 1 mB oxygen per second from the chest chemical tank | Water breathing. The chest tank holds 8,000 mB |
| `nutrition_unit` | Helmet | 1 | None | Feeds the wearer from one stored food stack |
| `energy_unit` | Chest | 8 | None | +1,000,000 J capacity per level |
| `jetpack_unit` | Chest | 1 | 50 J/t and 1 mB hydrogen per tick while thrusting | Conflicts with `grav_unit` |
| `grav_unit` | Chest | 1 | 200 J/t while flight is active | Creative-style flight. No hydrogen. Conflicts with `jetpack_unit` |
| `radiation_unit` | Chest | 2 | None | Level 1 adds 0.5 resistance. Level 2 adds 1.0. The radiation cap is 1 |
| `charge_unit` | Chest | 1 | Up to 1,000 J/t moved, not spent | Fills the tool from the chest |
| `dosimeter_unit` | Chest | 1 | None | Shows the radiation scale on the HUD |
| `speed_unit` | Legs | 4 | 20 J/t while moving | +20% walk speed per level |
| `jump_unit` | Legs | 3 | 30 J per jump | Higher jump per level |
| `fall_unit` | Boots | 1 | 40 J on a landing from 4 or more blocks | Negates that fall's damage |
| `magnet_unit` | Boots | 1 | 5 J per item pulled | 4 block radius |
| `silk_unit` | Tool | 1 | The block cost | Silk Touch. Conflicts with `fortune_unit` |
| `fortune_unit` | Tool | 3 | The block cost | Fortune level equal to the module level. Conflicts with `silk_unit` |
| `efficiency_unit` | Tool | 4 | Multiplies the block cost | Cost multiplier is `1 + level` |
| `vein_unit` | Tool | 3 | The block cost for every block in the vein | Breaks up to `16 * level` connected ore blocks |
| `attack_unit` | Tool | 4 | 100 J per hit | +2 damage per level. The tool's base damage is 8 |
| `farming_unit` | Tool | 1 | 20 J per use | Till and harvest |
| `shears_unit` | Tool | 1 | 20 J per use | Shears |

Hydrogen for the suit jetpack sits in the chest tank of 32,000 mB. Oxygen for breathing sits in the 8,000 mB tank. They are separate tanks.

## Installation

The modification station runs one install in 200 ticks at 1,000 J/t. It refuses the install, and does not consume the item, when:

- The piece does not accept that module.
- The piece already has 8 types and this type is new.
- The module is already at max level.
- Another enabled module is in its conflict set.

Removal returns one module item per level. A disabled module can be removed the same way. The station block uses the security rules in [CONTROL_SYSTEM.md](CONTROL_SYSTEM.md). The gear item itself has no owner.

The chest GUI and the tool GUI list modules, energy, and the conflict reason. [GEAR_GUI.md](GEAR_GUI.md) is the layout.

## When energy runs out

Flight stops on the tick the chest cannot pay `grav` or `jetpack`. The player falls. `fall_unit` still pays 40 J if the chest has it. If it does not, vanilla fall damage applies.

A vein stops before the block it cannot afford. Blocks already broken stay broken. An attack that cannot pay 100 J deals the base 8 damage and does not add the module bonus.

## Save

Each piece saves `mekanika:data_version` (1), `energy`, and a list of `{id, level, enabled}`. The chest also saves `hydrogen_mb` and `oxygen_mb`. A version newer than 1 clears modules and energy and logs an error.

## HUD

While the chest is worn, the hotbar shows the chest energy and an icon for each enabled module that drew energy last tick. The dosimeter module adds the radiation scale from the HUD doc. Radiation resistance shown is the worn total after the cap.

## Crafts

Station craft: 2 ultimate circuits + 4 atomic alloy + 1 enrichment chamber.

Each module item: 2 atomic alloy + 1 ultimate circuit. Installing it is what sets the level. The first install is level 1.

Each MekaSuit piece and the Meka-Tool: 4 atomic alloy + 1 ultimate circuit + 1 energy tablet. The tool does not consume a disassembler.

## Worked-example index

```normative-examples
example.energy_unit.full_capacity=9000000
example.jetpack.ticks_on_base_chest=20000
example.jetpack.hydrogen_limits_full_chest=32000
example.grav.ticks_on_base_chest=5000
example.radiation.level2=1.0
example.vein.level3.blocks=48
example.vein.level3.efficiency1.joules=9600
example.conflict.silk_fortune=refused
example.station.joules=200000
```
