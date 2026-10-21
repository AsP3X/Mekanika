# Control System API — Clean-Room Design

This document defines the conceptual design for Mekanika’s control systems, including redstone control, security modes, computer/peripheral integration, and the Configurator tool. All content is original.

## Overview

Machines and structures support multiple control layers:
- Redstone control (activation modes).
- Security modes (access control).
- Computer methods (for CC: Tweaked or similar).
- Configurator tool for side configuration and other modes.
- Logic adapters for advanced multiblock control.

## Redstone Control

The saved mode is `ignored`, `high`, `low`, or `pulse`. The GUI button cycles those four. A missing key loads as `ignored`. The public names for the same four modes are disabled, high, low, and pulse.

The signal is the strongest redstone level on the six neighbors, from 0 through 15. Powered means the signal is greater than 0. The block stores the previous signal in `mekanika:redstone_level`. On load that key is set to the current signal, so the first tick is not a rising edge.

| Mode | Runs |
| --- | --- |
| `ignored` | Always |
| `high` | Signal is greater than 0 |
| `low` | Signal is 0 |
| `pulse` | One armed operation |

A rising edge is a tick whose signal is greater than 0 and whose previous signal was 0. That edge sets `mekanika:pulse_armed`. A high signal that stays high does not arm again. The arm clears when the operation finishes. A second rising edge while the arm is set does not queue another operation.

One operation is:

- A batch machine or one factory slot: progress returns to 0 after an output. On a factory, only the lowest index that can run advances, and the arm clears when that slot finishes.
- A flow machine, a tank eject, a cube push, a solar or wind tick, or a multiblock tick: one tick.
- A heat or bio generator: the burn of one fuel item.
- A gas generator: one millibucket.

While the mode disallows running, status is `redstone_off` and the block does not process. A cube or tank still accepts inserts. Bucket slots still move. Fission `redstoneSetsInsertion` still writes `insertion = signal * 100 / 15` on every tick that flag is on.

An enrichment chamber in `pulse`, armed on tick 1, finishes its 200-tick batch at the end of tick 200. A signal held at 15 the whole time completes one batch. The arm is clear after that. A new rising edge is required for a second batch.

### Machine comparator

Structures, generators, and the QIO dashboard keep the formulas in their own docs. The twelve machines, their factories, the charge pad, tanks, and energy cubes use this one.

While any batch progress is above 0, the reading is `floor(progress * 15 / ticksRequired)`. A factory uses the greatest slot progress. Otherwise the reading is `floor(stored * 15 / capacity)`, or 0 when the capacity is 0.

Enrichment progress 100 of 200 reads `floor(1500 / 200) = 7`. A full 20,000 J buffer at progress 0 reads 15. An empty buffer reads 0. A charge pad at 10,000 of 20,000 reads 7.

## Security Modes

- Modes:
  - Public: Anyone can access.
  - Private: Only the owner can access.
  - Trusted: Owner and players on the trusted list can access.
- Owner is set on placement.
- Trusted players are managed via a Security Desk.
- An operator is a player whose permission level is 2 or higher. `general.operator_bypass` defaults to false. `general.creative_bypass` defaults to false. Either flag treats that player as the owner for GUI, break, insert, extract, computer methods, and QIO frequencies. The stored owner does not change.
- Unauthorized players cannot open GUIs, break the machine, or interact with it. The denial is `Access denied.` A bypass does not skip the sneak-to-void break on a controller that still holds energy or a tank.
- GUI lock icon shows and allows cycling modes (owner only).

## Computer / Peripheral Integration

- Machines and multiblocks expose the method table in [PUBLIC_API_SUMMARY.md](PUBLIC_API_SUMMARY.md) when a computer mod is loaded.
- A failed security check throws `Access denied.`
- `scram()` is fission and boiler only.

## Configurator Tool

- Multi-mode tool:
  - Configurate: Cycle side configuration modes.
  - Wrench: Rotate machines.
  - Optional: Network reader, security, etc.
- Right-click reports current mode; Shift + right-click cycles.
- Works on machines, transmitters, and multiblock ports.
- Active transmission type can be selected.

## Logic Adapters

Three blocks. Each replaces one face casing and is not a port. An unformed adapter emits 0. A formed adapter emits 0 or 15 from its outside face. Sneak-use cycles the mode and prints its English name. The mode is `mekanika:logic_mode`. Missing means `disabled`. There is no damage counter, so none of these modes is a damage alarm. A fission scram is reported by the heat mode.

| Block | Modes |
| --- | --- |
| `mekanika:fission_logic_adapter` | `disabled`, `activation`, `heat`, `waste`, `depleted` |
| `mekanika:fusion_logic_adapter` | `disabled`, `ready`, `capacity`, `depleted` |
| `mekanika:boiler_logic_adapter` | `disabled`, `heat`, `scram` |

`disabled` emits 0.

Fission `activation` is an input. While the outside signal is greater than 0 the reactor may burn. While it is 0 the reactor is `redstone_off`. Insertion is unchanged.

Fission `heat` emits 15 when the reactor is scrammed or `heat * 2 >= maxHeat`. One assembly has `maxHeat` 20,000, so heat 10,000 emits 15 and heat 9,999 emits 0.

Fission `waste` emits 15 when `wasteMb * 10 >= wasteCapacity * 9`. One assembly's waste capacity is `max(8000, 4000) = 8000`, so 7,200 emits 15 and 7,199 emits 0.

Fission `depleted` emits 15 when the fuel tank amount is 0.

Fusion `ready` emits 15 when the state is `idle` and the ignition buffer is at least 10,000,000 J. A buffer of 9,999,999 emits 0.

Fusion `capacity` emits 15 when `outputBuffer * 2 >= 2000000`. An output buffer of 1,000,000 emits 15. A buffer of 999,999 emits 0.

Fusion `depleted` emits 15 when the deuterium tank or the tritium tank is 0.

Boiler `heat` emits 15 when `displayDeciC >= 4500` or the boiler is scrammed. Capacity 1,000 and heat 896 display 4,500 and emit 15. Heat 895 displays 4,496 and emits 0.

Boiler `scram` emits 15 during the 600-tick vent and emits 0 otherwise.

## Normative constants

```normative
redstone.modes=4
redstone.signal_max=15
redstone.pulse.one_operation=1
redstone.comparator.scale=15
logic.adapter.high=15
logic.adapter.low=0
logic.fission.waste_num=9
logic.fission.waste_den=10
logic.blocks=3
```

## Worked-example index

```normative-examples
example.modes=4
example.default.runs=1
example.high.unpowered=0
example.low.unpowered=1
example.pulse.batches=1
example.comparator.progress=7
example.comparator.full=15
example.comparator.empty=0
example.comparator.charge_pad=7
example.fission.heat_on=10000
example.fission.heat_off=9999
example.fission.waste_on=7200
example.fission.waste_off=7199
example.fusion.ready_on=10000000
example.fusion.ready_off=9999999
example.fusion.capacity_on=1000000
example.fusion.capacity_off=999999
example.boiler.heat_on=896
example.boiler.heat_off=895
example.adapter.high=15
example.blocks=3
example.operator.level=2
example.operator_bypass.default=0
example.creative_bypass.default=0
example.op_private_off=0
example.op_private_on=1
example.level1_on=0
example.creative_off=0
example.creative_on=1
example.owner_off=1
```

## Implementation Recommendations

1. Implement redstone control as a reusable component early.
2. Add security modes with owner tracking and trusted list support.
3. Expose a clean set of computer methods for key machines and structures.
4. Make the Configurator a multi-mode tool from the start.
5. Support logic adapters for nuclear and large structures.
6. Document all computer methods clearly for players and addon authors.
7. Rate-limit computer method calls if needed to prevent lag.
8. Ensure security checks are applied to computer methods and GUI access.

## Edge Cases

- A held signal arms one pulse. The next arm needs the signal to return to 0 for a tick.
- Security changes while a player has a GUI open.
- Computer methods called on unformed multiblocks.
- Trusted list updates not immediately reflected.
- With `general.operator_bypass` false, a permission level of 2 on a private block is still `Access denied.` With the flag true, that player is allowed. A permission level of 1 stays denied. Creative mode is allowed only when `general.creative_bypass` is true. The owner is allowed while both flags are false.
- Logic adapter signals during structure formation/invalidation.

## Player Feedback

- Clear GUI indicators for redstone control and security modes.
- Messages when access is denied.
- Debug tool shows control state and last computer method calls (optional).
- Status lights or comparator output for key conditions.

This control system provides flexible automation and access control while remaining consistent across machines and structures.
