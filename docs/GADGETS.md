# Gadgets

Items that fill the gap before a MekaSuit. They use the same joule and the same hydrogen numbers as the modules in [GEAR_MODULE_SYSTEM.md](GEAR_MODULE_SYSTEM.md). A gadget in an armor or hand slot does nothing while the MekaSuit piece or Meka-Tool for that slot is worn.

The configurator's modes stay in [CONTROL_SYSTEM.md](CONTROL_SYSTEM.md). The dosimeter's readout stays in [RADIATION_SYSTEM.md](RADIATION_SYSTEM.md).

## Normative constants

```normative
gadget.disassembler.buffer=200000
gadget.disassembler.tier_mult.basic=1
gadget.disassembler.tier_mult.advanced=3
gadget.disassembler.tier_mult.elite=8
gadget.disassembler.tier_mult.ultimate=16
gadget.disassembler.slow_joules=20
gadget.disassembler.normal_joules=100
gadget.disassembler.fast_joules=400
gadget.jetpack.buffer=100000
gadget.jetpack.joules_per_tick=50
gadget.jetpack.hydrogen_mb_per_tick=1
gadget.jetpack.tank_mb=8000
gadget.free_runners.speed_bonus=0.3
gadget.lined.resistance_per_piece=0.2
```

## Items

| Item | Tier gate | Behavior |
| --- | --- | --- |
| `mekanika:configurator` | Basic circuit | Wrench, rotate, and side-config modes. No energy |
| `mekanika:dosimeter` | Basic circuit | Prints Sv/h and the scale name. No energy |
| `mekanika:free_runners` | 2 infused alloy | Boots. +30% walk speed. No energy. Ignored when MekaSuit boots are worn |
| `mekanika:atomic_disassembler` | 2 infused alloy, 1 advanced circuit, 1 energy tablet | Hand tool. Buffer 200,000 J times the tier multiplier |
| `mekanika:jetpack` | 2 reinforced alloy, 1 elite circuit, 1 energy tablet | Chest. Buffer 100,000 J. Hydrogen tank 8,000 mB |
| Lined suit pieces | 4 reinforced alloy each. The chest also takes 1 elite circuit | Armor. 0.2 resistance each. Four pieces are 0.8 |

Disassembler modes, cycled by sneak-use:

| Mode | Joules per block | Pace |
| --- | --- | --- |
| Off | 0 | Does not break blocks |
| Slow | 20 | Stone-tool pace |
| Normal | 100 | Iron-tool pace |
| Fast | 400 | Netherite-tool pace |

The tier multiplier changes the buffer only. A basic disassembler in Normal mode runs `200000 / 100 = 2000` blocks from a full buffer. Ultimate is 16 times that buffer, so 32,000 blocks.

The gadget jetpack matches the suit jetpack's 50 J/t and 1 mB hydrogen per tick of thrust. Energy runs out first: `100000 / 50 = 2000` ticks, with 6,000 mB hydrogen left in a tank that started full.

Free runners do not reduce fall damage. The suit's `fall_unit` is the fall cancel.

## Save

Disassembler saves `energy` and `mode`. Jetpack saves `energy` and `hydrogen_mb`. Both save `mekanika:data_version` = 1. A newer version clears energy and hydrogen.

## Worked-example index

```normative-examples
example.disassembler.normal_blocks=2000
example.disassembler.ultimate_buffer=3200000
example.jetpack.ticks=2000
example.jetpack.hydrogen_left_mb=6000
example.lined.four_pieces=0.8
example.free_runners.speed_bonus=0.3
```
