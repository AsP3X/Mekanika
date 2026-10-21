# Multiblock Structures

Canonical structure rules for the eight Mekanika multiblocks. The validation sheet in [MULTIBLOCK_SPECS.md](MULTIBLOCK_SPECS.md) repeats the constants, tick order, and worked examples. The numbers live in the normative block below. Formation phases live in [MULTIBLOCK_FORMATION.md](MULTIBLOCK_FORMATION.md). Invalidation timing lives in [MULTIBLOCK_INVALIDATION.md](MULTIBLOCK_INVALIDATION.md).

GUI documents describe layout. This document defines the values those screens read.

## Normative constants

```normative
multiblock.max_exterior=18
multiblock.max_exterior_clamp_min=3
multiblock.formation_period_ticks=10

tank.min=3
tank.max=18
tank.capacity_per_block_mb=16000
tank.dump_rate_mb_per_tick=4000

evap.footprint=4
evap.min_height=3
evap.max_height=18
evap.brine_per_lithium_mb=10
evap.lithium_per_cycle_mb=1
evap.progress_threshold=120000
evap.efficiency_step_permille=250
evap.rain_divisor=2
evap.input_capacity_mb_per_height=16000
evap.output_capacity_mb_per_height=8000

boiler.min=3
boiler.max=18
boiler.water_per_block_mb=16000
boiler.heat_buffer_per_element=1000
boiler.display_ambient_deci_c=200
boiler.display_span_deci_c=4800
boiler.boil_display_deci_c=1000
boiler.coolant_display_deci_c=4500
boiler.scram_display_deci_c=5000
boiler.scram_ticks=600
boiler.steam_mb_per_element_per_tick=4
boiler.heat_cost_per_steam_mb=2
boiler.max_heat_in_per_element=40
boiler.coolant_mb_per_tick_cap=10
boiler.heat_removed_per_coolant_mb=50
boiler.scram_vent_per_tick=100

sps.allowed_sizes=3,5,7
sps.energy_per_coil_j=1000000
sps.input_per_coil_j_per_tick=10000
sps.recipe_input_spent_fuel_mb=1000
sps.recipe_energy_j=1000000
sps.recipe_output_isomer_gas_mb=1
sps.min_operation_ticks=20
sps.tank_capacity_mb=64000

matrix.min=3
matrix.max=18
matrix.cell_basic_j=1000000000
matrix.cell_advanced_j=8000000000
matrix.cell_elite_j=64000000000
matrix.cell_ultimate_j=512000000000
matrix.provider_basic_j_per_tick=16000
matrix.provider_advanced_j_per_tick=64000
matrix.provider_elite_j_per_tick=256000
matrix.provider_ultimate_j_per_tick=1024000

turbine.min=5
turbine.max=17
turbine.flow_per_blade_mb_per_tick=200
turbine.flow_per_vent_mb_per_tick=400
turbine.base_joules_per_mb=10
turbine.extra_joules_per_100_deci_above_100c=1
turbine.output_per_coil_j_per_tick=5000
turbine.buffer_ticks=20
turbine.reference_steam_deci_c=1000

fission.min=3
fission.max=18
fission.milli_mb_per_assembly_at_insertion_0=1000
fission.water_mb_per_fuel_mb=10
fission.steam_mb_per_fuel_mb=10
fission.waste_mb_per_fuel_mb=1
fission.heat_per_fuel_mb=100
fission.max_heat_per_assembly=20000
fission.passive_cool_divisor=200
fission.tank_mb_per_assembly=4000
fission.tank_mb_minimum=8000
fission.radiation_at_insertion_0_per_assembly=100
fission.waste_radiation_divisor=1000
fission.scram_radiation_per_assembly=1000
fission.steam_bonus_deci_span=4000

fusion.size=5
fusion.ignition_energy_j=10000000
fusion.idle_input_j_per_tick=50000
fusion.hold_ticks=100
fusion.fuel_mb_per_tick=2
fusion.output_j_per_tick=1000000
fusion.output_buffer_j=2000000
fusion.quench_ticks=20
fusion.running_radiation=2
fusion.break_radiation=500
fusion.tank_capacity_mb=64000
```

Server config may set `multiblock.max_exterior` from 3 through 18. The default is 18. A structure's own maximum is the lesser of its normative maximum and this key. Fusion forms only when the key is at least 5. Evaporation forms only when the key is at least 4. Turbine dimensions stay odd and at most 17.

`multiblock.formation_period_ticks` is fixed at 10. That is twice per second. Controllers attempt formation at most once per period while unformed or dirty. The period is the cooldown after a failed attempt.

A formed structure that no longer fits `multiblock.max_exterior` after a config reload unforms on its next validation and reports `too_large`. Lowering the key does not delete stored resources on the controller.

A saved `mekanika:data_version` newer than 1 is refused. The controller keeps its block, clears formed state, and logs an error. It does not guess a migration.

## Shared rules

### Coordinates

Positions are block coordinates. Bounds are inclusive. Exterior length on an axis is `max - min + 1`.

A position is on the frame when at least one axis sits on `min` or `max`. A position is on an edge when it is on the frame and at least two axes sit on a bound. Face positions are the frame positions that are not edges. Interior positions have every axis strictly inside the bounds.

Structures are axis-aligned. Block facing is stored for rendering and is ignored by validation.

### Discovery

Each structure has a set of frame block ids, listed in its chapter. Discovery starts at the block that triggered the attempt and flood-fills through the six face neighbors that are frame blocks of that same structure. The fill does not cross interior blocks.

The bounding box of the filled set is the candidate. The attempt fails with `not_cuboid` when any filled block lies outside the mathematical frame of that box, or when any frame position of that box is missing from the fill. It fails with `frame_hole` when a frame position is empty air. It fails with `edge_block` or `face_block` when the block id is illegal for that slot.

`chunk_unloaded` is returned when any chunk of the candidate box is unloaded. That result is logged at debug and is not sent to chat. The controller retries on a later period.

A block belongs to at most one formed structure. A candidate that includes a block already claimed by another formed structure fails with `already_formed` at that position.

Exactly one controller block may sit in the fill. Zero fails with `missing_controller`. Two or more fails with `multiple_controllers`. A controller on an edge fails with `controller_on_edge`. Contents stay on the controller block entity. Mekanika does not mix the contents of two controllers when shells touch.

Placing, breaking, or replacing an interior block marks a formed structure dirty. The next period runs a full validation because those blocks change capacity, flow, or burn rate. None of these eight structures has an activation state that skips that check.

### What is a block entity

The controller is a block entity. Valves, ports, and the fusion fuel port are block entities because they store side configuration.

Casing, glass, solar panels, vents, lasers, the fusion core, coils, rotors, blades, cells, providers, fuel assemblies, control rods, superheating elements, and pressure dispersers are blocks. Their contribution is read from the block id. They do not tick.

Every non-controller block in a formed structure stores the controller position and opens the controller GUI on use. The controller owns `MultiblockData`.

### Air and entities

Interior rules that demand air accept any block state for which `isAir()` is true, including cave air. Fluids, plants, and other replaceable blocks fail the interior check. Entities never block formation. Formed interiors stay air. Stored fluid, chemical, and energy are values on the controller, not world blocks.

Casings, glass, valves, and ports are not waterloggable.

### Side configuration

Ports use the shared side-configuration component from [SIDE_CONFIGURATION.md](SIDE_CONFIGURATION.md). Each port chapter lists the modes that port actually offers. Unsupported transmission types are hidden.

Port scan order, used wherever a budget is shared, is ascending Y, then ascending Z, then ascending X.

### Comparator and redstone

Unless a chapter says otherwise, the comparator reading on the controller and on every port is `floor(value * 15 / max)`, or 0 when `max` is 0. Redstone control uses the standard modes in [CONTROL_SYSTEM.md](CONTROL_SYSTEM.md). Security modes on the controller match that document. A config card copies side configuration, redstone mode, and the settings named in each chapter. It does not copy tank contents or stored energy.

### Breaking a controller

When stored energy or any tank amount is greater than zero, the controller block breaks only while the player is sneaking. A normal break is cancelled and sends `message.mekanika.multiblock.contents_protected`. A sneaking break voids those stored resources. Heat, recipe progress, and scram flags are not resources and do not require sneaking. Security can still refuse the break.

Breaking any other block unforms the structure and leaves the controller, and its contents, in the world.

### Steam temperature

Steam is the chemical `mekanika:steam`. Each stored steam stack carries `temperature_deci_c`. A missing value means 1000, which is 100.0°C. Merging two steam amounts uses integer math:

`temperature = (amountA * tempA + amountB * tempB) / (amountA + amountB)`

When the combined amount is 0, the temperature is 1000. Chemical tubes that carry steam keep this value beside the amount. The turbine reads it from the steam it accepts.

### Heat acceptance

Boiler casings implement `acceptHeat(offered) -> accepted`. The call only queues heat. It does not boil on its own. The queue limit is in the boiler chapter. Conductor behavior itself belongs to the heat-system document. Fission, turbines, tanks, evaporation plants, SPS blocks, matrices, and fusion reactors do not accept conductor heat.

### Save and sync

Each controller saves `mekanika:data_version` (integer, currently 1), the formed flag, side configuration, redstone and security settings, and the fields in its chapter. Positions, dimensions, and aggregated rates are recalculated on load. If a recalculated capacity is below a saved amount, the extra is deleted and the owner receives `message.mekanika.multiblock.energy_voided` when the deleted value is energy. The same event name is used for a tank overflow after a shrink, with the tank named in the message argument.

Formation changes sync on the tick they happen. Tank amounts, energy, heat, and rates sync at most every 10 ticks, and immediately when a GUI is opened. Every controller also syncs the last failure code and the failure position so the multiblock data screen can show them.

### Failure chat

Formation failures other than `chunk_unloaded` send one chat line to players within 8 blocks and to the security owner. The line includes the localized reason and the block position. A repeat of the same reason and position is suppressed until the structure forms or the reason changes.

| Code | Key | English |
| --- | --- | --- |
| `too_small` | `message.mekanika.multiblock.too_small` | Structure is too small at %s |
| `too_large` | `message.mekanika.multiblock.too_large` | Structure is too large at %s |
| `not_cuboid` | `message.mekanika.multiblock.not_cuboid` | Structure is not a rectangular shell at %s |
| `not_square` | `message.mekanika.multiblock.not_square` | Footprint must be square at %s |
| `wrong_parity` | `message.mekanika.multiblock.wrong_parity` | Dimensions must be odd at %s |
| `wrong_fixed_size` | `message.mekanika.multiblock.wrong_fixed_size` | Structure must be %s at %s |
| `frame_hole` | `message.mekanika.multiblock.frame_hole` | Frame is incomplete at %s |
| `edge_block` | `message.mekanika.multiblock.edge_block` | Edge must be casing at %s |
| `face_block` | `message.mekanika.multiblock.face_block` | Illegal face block at %s |
| `missing_controller` | `message.mekanika.multiblock.missing_controller` | Structure needs one controller |
| `multiple_controllers` | `message.mekanika.multiblock.multiple_controllers` | Structure has more than one controller at %s |
| `controller_on_edge` | `message.mekanika.multiblock.controller_on_edge` | Controller must sit on a face at %s |
| `interior_block` | `message.mekanika.multiblock.interior_block` | Interior block is illegal at %s |
| `missing_component` | `message.mekanika.multiblock.missing_component` | Required component missing at %s |
| `component_placement` | `message.mekanika.multiblock.component_placement` | Component is in the wrong place at %s |
| `fuel_gap` | `message.mekanika.multiblock.fuel_gap` | Fuel column has a gap at %s |
| `uncontrolled_fuel` | `message.mekanika.multiblock.uncontrolled_fuel` | Fuel column needs a control rod at %s |
| `rotor_gap` | `message.mekanika.multiblock.rotor_gap` | Rotor is missing at %s |
| `blade_gap` | `message.mekanika.multiblock.blade_gap` | Blade is not connected to the rotor at %s |
| `vent_missing` | `message.mekanika.multiblock.vent_missing` | Top vent is missing at %s |
| `solar_missing` | `message.mekanika.multiblock.solar_missing` | Solar panel is missing at %s |
| `coil_placement` | `message.mekanika.multiblock.coil_placement` | Coil must fill the center column at %s |
| `already_formed` | `message.mekanika.multiblock.already_formed` | Block already belongs to a structure at %s |
| `contents_protected` | `message.mekanika.multiblock.contents_protected` | Sneak to break this controller and void its contents |
| `energy_voided` | `message.mekanika.multiblock.energy_voided` | Stored %s exceeded the new capacity and the extra was removed |
| `boiler_scram` | `message.mekanika.multiblock.boiler_scram` | Boiler emergency vent at %s |
| `fission_scram` | `message.mekanika.multiblock.fission_scram` | Fission reactor scrammed at %s |
| `fusion_quench` | `message.mekanika.multiblock.fusion_quench` | Fusion reaction quenched at %s |

Sound events fired by these rules: `mekanika.multiblock.form`, `mekanika.multiblock.fail`, `mekanika.boiler.scram`, `mekanika.fission.scram`, `mekanika.fusion.ignite`, `mekanika.fusion.quench`. Particle placement follows [SOUNDS_AND_PARTICLES.md](SOUNDS_AND_PARTICLES.md): a short highlight on success, and an error particle on the failure position.

## Dynamic Tank

Stores one fluid or one chemical. The interior stays air.

### Shape

Each exterior side is from 3 through 18, and not above `multiblock.max_exterior`. The tank does not have to be a cube. Interior volume is `(width - 2) * (height - 2) * (length - 2)`.

### Blocks

| Id | Where |
| --- | --- |
| `mekanika:dynamic_tank_casing` | Edges, and any face that is not glass, a valve, or the controller |
| `mekanika:dynamic_tank_glass` | Faces |
| `mekanika:dynamic_tank_valve` | Faces |
| `mekanika:dynamic_tank_controller` | One face position |

Edges accept casing only. The interior accepts air only.

### Capacity

`capacityMb = interiorVolume * tank.capacity_per_block_mb`

A 3×3×3 tank has interior volume 1 and holds 16,000 mB. A 5×4×6 tank has interior volume 24 and holds 384,000 mB.

### Contents

The tank has one mode: empty, fluid, or chemical. The first successful execute insert locks the mode. Simulate does not lock it. The other handler rejects inserts until the amount is 0, which returns the mode to empty. The player may also lock a type from the GUI before anything is inserted.

Valves and the controller expose the handler for the active mode. Modes are Input, Output, Both, and None. The default is Both.

### Dump, eject, and buckets

Auto-eject is off by default. When it is on, Output and Both faces push, in port scan order, up to `tank.dump_rate_mb_per_tick` altogether.

Dump mode is one of:

- None. The tank only stores.
- Excess. Amount above the GUI target is deleted at the dump rate. The default target is the full capacity, so Excess deletes nothing until the target is lowered.
- All. The contents are deleted at the dump rate.

The controller GUI has one fill slot and one drain slot for buckets and chemical containers. Valves do not have item slots.

### Tick

The tank does not process. Inserts, ejects, and dumps happen when handlers are called and once per controller tick for eject and dump. The comparator reads stored amount over capacity.

### Save

Mode, optional lock id, fluid id and amount, chemical id and amount, dump mode, dump target, auto-eject flag. Capacity is recalculated from the volume.

### GUI fields

Amount, capacity, type, dump mode, auto-eject, valve list. These extend the multiblock data screen as described in [TANK_GUI.md](TANK_GUI.md).

### Runtime status

`empty`, `holding`, `dumping`. Formation errors use the shared catalog.

## Thermal Evaporation Plant

A fixed-footprint tower. Brine fluid enters. Lithium chemical leaves. Height and sunlight set the rate. The default recipe is data, so another pair can replace brine and lithium.

### Shape

Exterior X and Z are both 4. Exterior height is from 3 through 18, limited by `multiblock.max_exterior`. Any other footprint fails `wrong_fixed_size`. Interior height `h = exteriorHeight - 2`, so `h` is 1 through 16. The interior is a 2×2×h column of air.

### Blocks

| Id | Where |
| --- | --- |
| `mekanika:evaporation_casing` | All edges, the entire bottom layer, and side faces that are not valves or the controller |
| `mekanika:evaporation_solar_panel` | The inner 2×2 of the top layer |
| `mekanika:evaporation_valve` | Side faces |
| `mekanika:evaporation_controller` | One side face |

The top rim stays casing. A missing solar panel fails `solar_missing`. The controller is not a valve.

### Sun

A solar panel is uncovered when every block above it, up to the build height, is air. Night is `dayTime % 24000 >= 12000`. Thunder, night, and any dimension without skylight set efficiency to 0. Rain divides efficiency by `evap.rain_divisor` after the step below.

`efficiencyPermille = uncoveredPanels * evap.efficiency_step_permille`

Uncovered panels are 0 through 4. Full sun on all four panels is 1000 permille.

### Recipe and tanks

Default datapack recipe `data/mekanika/recipe/evaporating/brine_lithium.json`:

```json
{
  "type": "mekanika:evaporating",
  "input": { "fluid": "mekanika:brine", "amount": 10 },
  "output": { "chemical": "mekanika:lithium", "amount": 1 }
}
```

The amounts in the file replace `evap.brine_per_lithium_mb` and `evap.lithium_per_cycle_mb` for that recipe. The progress threshold stays in config. When several recipes match the fluid on hand, the plant uses the one whose recipe id sorts first as a string.

`inputCapacity = h * evap.input_capacity_mb_per_height`

`outputCapacity = h * evap.output_capacity_mb_per_height`

### Tick

Each formed tick adds `h * efficiencyPermille` to `progress`. While `progress` is greater than or equal to `evap.progress_threshold`, the input covers one recipe, and the output tank can take one recipe output, subtract one threshold, consume one input, and emit one output, then check again. Default numbers add at most 16,000 per tick against a 120,000 threshold, so a default plant completes at most one cycle per tick. When a cycle cannot complete, `progress` is reduced to the threshold so a stalled plant does not store an unbounded backlog. The worked case below has brine and lithium room, so the leftover after the subtraction stays.

Valve default, applied once when the valve is placed: `midY = minY + exteriorHeight / 2`, using integer division. A valve with Y below `midY` starts as Brine Input. A valve at or above `midY` starts as Lithium Output. The player can change it. Modes are Brine Input, Lithium Output, and None.

The comparator reads the lithium tank fill.

### Worked sun

| Case | Result |
| --- | --- |
| `h = 1`, 4 panels, clear day | +1000 progress per tick. One lithium mB every 120 ticks. 10 mB brine consumed. |
| `h = 16`, 4 panels, clear day | +16000 per tick. After 7 ticks, progress 112000 and lithium 0. After 8 ticks, lithium 1 and progress 8000. |
| `h = 16`, 2 panels, or 4 panels in rain | +8000 per tick. One lithium mB every 15 ticks. |

### Save

Progress, brine amount, lithium amount, selected recipe id when the player pinned one. Height and efficiency are recalculated.

### GUI fields

Brine amount and capacity, lithium amount and capacity, exterior height, interior height, uncovered panels, efficiency permille, last production rate. Matches [BOILER_EVAP_SPS_GUI.md](BOILER_EVAP_SPS_GUI.md).

### Runtime status

`no_sun`, `missing_brine`, `output_full`, `producing`, `missing_valve` when no brine valve or no lithium valve exists. The structure still forms in that last case.

## Thermoelectric Boiler

Turns water and heat into steam. Superheating elements sit on the bottom interior layer. Pressure dispersers sit on the top interior layer when the interior is at least two blocks tall. The cavity between them holds the water volume.

### Shape

Each exterior side is from 3 through 18, limited by the server max. Interior sizes are `ix`, `iy`, `iz`.

### Blocks

| Id | Where |
| --- | --- |
| `mekanika:boiler_casing` | Edges, and faces that are not glass, valves, the controller, or the logic adapter |
| `mekanika:boiler_logic_adapter` | One face, not an edge |
| `mekanika:boiler_glass` | Faces |
| `mekanika:boiler_valve` | Faces |
| `mekanika:boiler_controller` | One face position |
| `mekanika:superheating_element` | Entire lowest interior layer |
| `mekanika:pressure_disperser` | Entire highest interior layer when `iy >= 2` |

When `iy` is 1, the single interior layer is elements and dispersers are absent. When `iy` is 2 or more, every middle interior layer is air. Any other interior block fails `interior_block`.

`E = ix * iz` elements. `D = ix * iz` when `iy >= 2`, otherwise 0.

Water blocks `B` equal `E` when the air count is 0, and equal the air count otherwise. A 3×3×3 boiler has `E = 1`, `D = 0`, and `B = 1`. A boiler with `iy = 2` has no air, so `B = E` as well.

### Tanks and heat

`waterCapacity = B * boiler.water_per_block_mb`

`steamCapacity = waterCapacity`

`sinkCapacity = waterCapacity`

The sink is a second water tank used only for emergency cooling.

`heatCapacity = E * boiler.heat_buffer_per_element`

`displayDeciC = 200 + heatBuffer * 4800 / heatCapacity` when capacity is positive. Empty buffer reads 20.0°C. A full buffer reads 500.0°C.

Boiling starts when `displayDeciC >= 1000`. For a capacity of 1000, that is heat 167 (display 1001). Heat 166 reads 996 and does not boil. Coolant assistance starts at display 4500, which is heat 896 when capacity is 1000.

### Ports

Valve modes are Water Input, Steam Output, Coolant Input, and None. The controller is not a fluid port. Casing queues heat through `acceptHeat`. Valves and the controller do not accept heat.

`acceptHeat` queues at most `E * boiler.max_heat_in_per_element` minus what is already queued this period, and it also refuses heat that would push `heatBuffer + pending` past `heatCapacity`. When `D` is 0 it refuses heat that would push the buffer past the boiling heat. Queued heat is committed at the start of the boiler tick.

### Tick

1. Commit `pendingHeat` into `heatBuffer` and clear the queue.
2. When the boiler is scrammed, subtract `boiler.scram_vent_per_tick` from the buffer, floored at 0, and skip production.
3. When `displayDeciC >= 1000`, water is present, and the steam tank has room: `steamMb = min(E * 4, waterMb, steamRoomMb, heatBuffer / 2)`. Consume that much water, add that much steam, subtract `steamMb * 2` heat. Steam temperature is 1000 when `D` is 0. Otherwise it is the display value from before this subtraction.
4. When display is at least 4500 and the sink has water: consume up to 10 mB and remove 50 heat per mB consumed.
5. When display is at least 5000, increment `scramTimer`. Otherwise reset it to 0. At 600 ticks, set `scrammed`, send `boiler_scram`, and play the scram sound. The player clears the flag from the GUI. The structure stays formed.

Pressure shown on the GUI is derived and is not fed back into the tick:

`pressureKpa = 100 + steamStored * 400 / steamCapacity + max(0, steamDeciC - 1000) / 10`

The comparator reads heat buffer fill.

### Worked 3×3×3

`D` is 0, so the buffer clamps to boiling heat 167. Offered heat is 40 at the start of each tick. Water and steam room are full.

| Tick | Accept | Steam | Heat after |
| --- | --- | --- | --- |
| 1 | 40 | 0 | 40 |
| 2 | 40 | 0 | 80 |
| 3 | 40 | 0 | 120 |
| 4 | 40 | 0 | 160 |
| 5 | 7 | 4 | 159 |
| 6 and later | 8 | 4 | 159 |

Steady production is 4 mB of 100.0°C steam per tick.

### Save

Heat buffer, scram flag, scram timer, water amount, steam amount, steam temperature, sink amount. Element and disperser counts are recalculated.

### GUI fields

Steam rate, heat display, water and sink amounts, steam amount, pressure, element count, disperser count, scram state, valve list.

### Runtime status

`warming`, `boiling`, `needs_water`, `steam_full`, `scrammed`, `missing_valve`.

### Config card

Copies valve modes and the redstone mode.

## Supercritical Phase Shifter

A small odd cuboid. The center column is coils. The default process turns `mekanika:spent_fuel` into `mekanika:isomer_gas`.

### Shape

Every exterior side is 3, 5, or 7, and not above the server max. Any other size fails `wrong_parity` when a side is even, or `too_small` / `too_large` when it is outside that set. Interior center column length equals `side - 2` coils: 1, 3, or 5.

### Blocks

| Id | Where |
| --- | --- |
| `mekanika:sps_casing` | Edges, and faces that are not ports or the controller |
| `mekanika:sps_port` | Faces |
| `mekanika:sps_controller` | One face position |
| `mekanika:sps_coil` | Every interior block on the center X and center Z |

Every other interior block is air. A coil off the center column, or air on the center column, fails `coil_placement`. Glass is not a frame block.

### Energy and tanks

`bufferCapacity = coilCount * sps.energy_per_coil_j`

`maxInputPerTick = coilCount * sps.input_per_coil_j_per_tick`

Input and output chemical tanks each hold `sps.tank_capacity_mb`.

The controller accepts energy. Its energy modes are Input and None. The default is Input. Ports do not move energy. Port modes are Chemical Input, Chemical Output, and None.

### Recipe

Default file `data/mekanika/recipe/sps/nuclear_isomer.json`:

```json
{
  "type": "mekanika:sps",
  "input_chemical": { "id": "mekanika:spent_fuel", "amount": 1000 },
  "energy": 1000000,
  "min_ticks": 20,
  "output_chemical": { "id": "mekanika:isomer_gas", "amount": 1 }
}
```

An operation starts when the input tank holds the recipe input and the output tank can hold the recipe output. Fuel is consumed only on completion. Each started tick accepts energy into the buffer up to `maxInputPerTick` and increments `operationTicks`. The operation completes when `operationTicks` is at least `min_ticks` and the buffer holds the recipe energy. Completion subtracts that energy, consumes the input, and emits the output. Leftover buffer energy stays. If the input is removed after the start, ticks and buffer stay where they are and completion waits.

The comparator reads energy buffer fill.

### Worked coils

| Coils | Input cap | Ticks to complete | Buffer left |
| --- | --- | --- | --- |
| 1 (3×3×3) | 10,000 J/t | 100 | 0 |
| 3 (5×5×5) | 30,000 J/t | 34 | 20,000 J |
| 5 (7×7×7) | 50,000 J/t | 20 | 0 |

### Save

Energy buffer, operation ticks, input amount, output amount, recipe id.

### GUI fields

Input and output rates, energy buffer, coil count, operation ticks, port list.

### Runtime status

`missing_input`, `output_full`, `charging`, `running`, `missing_port`.

## Induction Matrix

Energy storage. Cells sum to capacity. Providers sum to the transfer budget. Mixed tiers are valid. The interior is filled completely.

### Shape

Each exterior side is from 3 through 18, limited by the server max.

### Blocks

| Id | Where |
| --- | --- |
| `mekanika:induction_casing` | Edges, and faces that are not glass, ports, or the controller |
| `mekanika:induction_glass` | Faces |
| `mekanika:induction_port` | Faces |
| `mekanika:induction_controller` | One face position |
| `mekanika:induction_cell_basic` | Interior |
| `mekanika:induction_cell_advanced` | Interior |
| `mekanika:induction_cell_elite` | Interior |
| `mekanika:induction_cell_ultimate` | Interior |
| `mekanika:induction_provider_basic` | Interior |
| `mekanika:induction_provider_advanced` | Interior |
| `mekanika:induction_provider_elite` | Interior |
| `mekanika:induction_provider_ultimate` | Interior |

Every interior position is one cell or one provider. Air fails `interior_block`.

### Defaults

| Tier | Cell capacity (J) | Provider rate (J/t) |
| --- | --- | --- |
| Basic | 1,000,000,000 | 16,000 |
| Advanced | 8,000,000,000 | 64,000 |
| Elite | 64,000,000,000 | 256,000 |
| Ultimate | 512,000,000,000 | 1,024,000 |

`capacity = sum of cell values`

`maxTransfer = sum of provider rates`

The controller stores the energy, clamped to capacity. A shrink that leaves energy above the new capacity deletes the extra and sends `energy_voided`.

### Ports

The controller and each port are energy connections. Modes are Input, Output, Both, and None. The default is Both. The transfer budget is shared by the whole structure per tick, for input and output together. Requests are served in port scan order until `maxTransfer` is spent. A matrix with cells and no providers forms and holds energy at a transfer of 0.

The comparator reads stored energy over capacity.

### Worked builds

| Interior | Capacity | Rate |
| --- | --- | --- |
| One basic cell (3×3×3) | 1,000,000,000 J | 0 |
| 18 basic cells and 9 basic providers | 18,000,000,000 J | 144,000 J/t |

### Save

Stored energy. Capacity and rate are recalculated from the interior blocks.

### GUI fields

Stored energy, capacity, last input, last output, max transfer, cell counts by tier, provider counts by tier. Matches [INDUCTION_MATRIX_GUI.md](INDUCTION_MATRIX_GUI.md).

### Runtime status

`formed`, `no_providers`, `no_cells`, `full`.

### Config card

Copies every port mode and the redstone mode.

## Industrial Turbine

Converts steam into energy. X and Z match, every side is odd, and the center column is the rotor. Vents occupy the whole inner top face.

### Shape

X equals Z. X, Y, and Z are odd and from 5 through 17, and not above the server max. A rectangle footprint fails `not_square`. An even side fails `wrong_parity`.

Interior width `n = X - 2`. Interior height `iy = Y - 2`. Vent count `V = n * n`.

### Blocks

| Id | Where |
| --- | --- |
| `mekanika:turbine_casing` | Edges, side faces that are not valves or the controller, and the bottom face |
| `mekanika:turbine_vent` | Every inner block of the top face |
| `mekanika:turbine_valve` | Side faces |
| `mekanika:turbine_controller` | One side face |
| `mekanika:turbine_rotor` | The entire interior center column |
| `mekanika:turbine_blade` | Interior, on a cardinal line from the rotor |
| `mekanika:turbine_coil` | Any other interior position |

A missing vent fails `vent_missing`. A missing rotor block fails `rotor_gap`. A blade is legal when the blocks from that blade back to the rotor, in the same Y layer, along one cardinal axis, are blades or the rotor, with no air gap and no diagonal step. Any other blade fails `blade_gap`. Coils may occupy the remaining interior, including the diagonal corners. Air may occupy those same remaining positions.

The structure forms with zero blades or zero coils. Output is then limited, as the status banner describes.

### Flow and energy

`bladeFlow = bladeCount * turbine.flow_per_blade_mb_per_tick`

`ventFlow = V * turbine.flow_per_vent_mb_per_tick`

`steamCap = min(bladeFlow, ventFlow)`

`joulesPerMb = 10 + max(0, steamDeciC - 1000) / 100`

100.0°C steam is 10 J per mB. 500.0°C steam is 50 J per mB.

`coilCap = coilCount * turbine.output_per_coil_j_per_tick`

`coilSteamCap = coilCap / joulesPerMb` when `joulesPerMb` is positive, otherwise 0. Zero coils force `coilSteamCap` to 0.

`steamBufferCapacity = steamCap * turbine.buffer_ticks`

`energyBufferCapacity = coilCap * turbine.buffer_ticks`

### Tick

Valves accept steam. The default mode is Input. The controller emits energy. Its modes are Output and None. The default is Output.

1. Pull steam from valves into the steam buffer, up to the free buffer space. Incoming temperature merges with the rule in the shared steam section.
2. `usable = min(steamInBuffer, steamCap)`
3. `actualSteam = min(usable, coilSteamCap)`
4. Remove `actualSteam` from the buffer and add `actualSteam * joulesPerMb` to the energy buffer, capped by the energy buffer capacity. Steam that does not fit in the energy budget is not consumed.

Status, first match wins:

1. No steam in the buffer at the start of the step: `steam-limited`
2. `coilSteamCap` is 0, or `coilSteamCap` is less than or equal to both flow caps and `actualSteam` equals `coilSteamCap` while steam was available: `coil-limited`
3. `bladeFlow <= ventFlow` and `actualSteam` equals `bladeFlow` while steam was at least that large: `blade-limited`
4. `actualSteam` equals `ventFlow` while steam was at least that large: `vent-limited`
5. Otherwise: `steam-limited`

When blade flow and vent flow are equal and both bind, the result is `blade-limited`. The GUI tokens are `forming`, `active`, `steam-limited`, `blade-limited`, `vent-limited`, `coil-limited`, and `offline`. `active` is the formed state while the limiting token is also shown. `offline` means unformed.

The comparator reads last-tick energy over `coilCap`.

### Worked turbines

Steam is plentiful and is 100.0°C unless noted.

| Build | Flows | Result |
| --- | --- | --- |
| 5×5×17, 60 blades (4 per layer, 15 layers), 9 vents, 20 coils | Blade 12,000, vent 3,600, coil steam cap 10,000 | 3,600 mB and 36,000 J, `vent-limited` |
| 5×5×5, 12 blades, 9 vents, 12 coils | Blade 2,400, vent 3,600, coil steam cap 6,000 | 2,400 mB and 24,000 J, `blade-limited` |
| 5×5×5, 12 blades, 0 coils, steam present | Coil steam cap 0 | 0 mB, `coil-limited` |

### Save

Steam amount, steam temperature, energy amount. Blade, coil, and vent counts are recalculated.

### GUI fields

Energy rate, steam rate, efficiency token, blade count, coil count, vent count, dimensions, limiting status. Matches [TURBINE_GUI.md](TURBINE_GUI.md).

### Config card

Copies valve modes and the controller energy mode.

## Fission Reactor

Burns `mekanika:fissile_fuel`, consumes water, and emits steam plus `mekanika:spent_fuel`. Electrical energy is produced by sending that steam to a turbine. The reactor GUI reports the energy that steam would make at the turbine's 100°C-and-above conversion. The reactor block does not push Joules onto cables.

### Shape

Each exterior side is from 3 through 18, limited by the server max.

### Blocks

| Id | Where |
| --- | --- |
| `mekanika:fission_casing` | Edges, and faces that are not glass, ports, the controller, a control rod, or the logic adapter |
| `mekanika:fission_logic_adapter` | One face, not where a control rod is required |
| `mekanika:fission_glass` | Faces, except where a control rod is required |
| `mekanika:fission_port` | Faces, except where a control rod is required |
| `mekanika:fission_controller` | One face position that is not a required control rod |
| `mekanika:fission_control_rod` | Top face, on the XZ of a fuel column |
| `mekanika:fission_fuel_assembly` | Interior |

A fuel column is one interior XZ. The Y positions in that column that contain fuel must be one contiguous range. A gap fails `fuel_gap`. Every column that contains fuel needs the top-face block of that same XZ to be a control rod. A missing rod fails `uncontrolled_fuel` on the fuel block. A control rod over an empty column is legal. Air is legal in the rest of the interior.

On a 3×3×3 the six face centers are the only face positions. The top center is the control rod for the single interior cell. One of the other five is the controller. The other four are the four port jobs.

### Burn

`maxHeat = fuelAssemblies * fission.max_heat_per_assembly`

Tank capacity for fuel, water, steam, and waste is `max(fission.tank_mb_minimum, fuelAssemblies * fission.tank_mb_per_assembly)`.

Insertion is an integer 0 through 100. 0 is fully withdrawn and is the fastest burn. 100 is fully inserted and requests no burn. The GUI sets it unless `redstoneSetsInsertion` is on, in which case the strongest redstone signal on the controller sets `insertion = signal * 100 / 15`.

Each tick, when the reactor is not scrammed and waste room is greater than 0:

`requestedMilli = fuelAssemblies * (100 - insertion) * 10`

Cap `requestedMilli` by the fuel on hand, measured in milli-mB. Add it to `burnAccumulator`. `burnMb = burnAccumulator / 1000`, then reduce `burnAccumulator` by `burnMb * 1000`. Also cap `burnMb` by waste room. Do not cap it by water or steam room.

Then:

- Consume `burnMb` fuel and emit `burnMb` waste.
- Add `burnMb * fission.heat_per_fuel_mb` heat.
- `cooledMb = min(burnMb, waterMb / 10, steamRoomMb / 10)`
- Consume `cooledMb * 10` water and emit `cooledMb * 10` steam.
- Subtract `cooledMb * fission.heat_per_fuel_mb` heat.

Net heat from a fully cooled mB is 0. Each uncooled mB adds 100 heat. A full waste tank pauses the burn. A full steam tank does not pause the burn. Missing water does not pause the burn.

Steam temperature is `1000 + min(4000, heat * 4000 / max(1, maxHeat))`.

When `burnMb` is 0 and the reactor is not mid-scram setup, `heat = heat - max(1, heat / 200)`, floored at 0.

After the burn and the passive-cool step, when `fuelAssemblies > 0` and `heat >= maxHeat`, set `scrammed`, force insertion to 100, emit one radiation burst of `fuelAssemblies * fission.scram_radiation_per_assembly`, send `fission_scram`, and play the scram sound. The player may clear scram from the GUI once heat is at most half of `maxHeat`.

When a rebuild leaves zero fuel assemblies, set heat to 0 and clear scram.

### Radiation contract

While a non-scrammed reactor burns, source strength at the controller is `fuelAssemblies * (100 - insertion)`. Stored waste adds `wasteMb / 1000`, integer division. Those integers are sieverts per hour. [RADIATION_SYSTEM.md](RADIATION_SYSTEM.md) turns them into dose. The scram burst is added once and then decays. It is not overwritten by the held strength.

### Potential energy

`steamEnergyJoules = steamMbThisTick * (10 + max(0, steamDeciC - 1000) / 100)`

The GUI label is "Energy in steam".

### Ports

Modes are Coolant In (water), Steam Out, Fuel In, Waste Out, and None. The structure forms with any subset. It burns only when all four jobs exist.

The comparator reads heat over `maxHeat`, and reads 15 while scrammed.

### Worked assembly

One assembly, insertion 0, fuel present, waste room present.

| Coolant | Each tick | Scram |
| --- | --- | --- |
| 10 mB water and steam room | 1 mB fuel, 10 mB water, 10 mB steam, 1 mB waste, net heat 0 | No |
| No water | 1 mB fuel, 1 mB waste, +100 heat, no steam | Tick 200, heat 20,000 |

Insertion 100 requests 0 milli-mB.

### Save

Heat, scram flag, insertion, redstone-sets-insertion flag, burn accumulator, four tank amounts, steam temperature.

### GUI fields

Fuel, water, steam, waste, heat, insertion, scram, radiation strength, steam energy, component counts. Matches [REACTOR_GUI.md](REACTOR_GUI.md). Fusion uses that same screen and hides the coolant, heat, and control-rod widgets.

### Runtime status

`idle`, `burning`, `uncooled`, `waste_full`, `missing_port`, `scrammed`.

### Config card

Copies port modes, insertion, and `redstoneSetsInsertion`.

### Fuel data

Additional fuels are JSON files under `data/mekanika/fission_fuel/`. A loaded file replaces the normative ratios for its fuel id. One fuel type may sit in the fuel tank at a time. The default file is `data/mekanika/fission_fuel/fissile_fuel.json`:

```json
{
  "fuel": "mekanika:fissile_fuel",
  "milli_mb_per_assembly_at_insertion_0": 1000,
  "water_mb_per_fuel_mb": 10,
  "steam_mb_per_fuel_mb": 10,
  "waste": "mekanika:spent_fuel",
  "waste_mb_per_fuel_mb": 1,
  "heat_per_fuel_mb": 100
}
```

## Fusion Reactor

A fixed 5×5×5 shell with four lasers, one fuel port, one controller, and a single core. It ignites on stored energy plus a fuel hold, then pushes Joules directly.

### Shape

Exterior size is 5 on every axis. Anything else fails `wrong_fixed_size`. When `multiblock.max_exterior` is below 5, fusion cannot form and the failure is `too_large`.

### Blocks

Face centers of a structure whose minimum corner is `(x, y, z)` are:

- `(x, y+2, z+2)` and `(x+4, y+2, z+2)`
- `(x+2, y, z+2)` and `(x+2, y+4, z+2)`
- `(x+2, y+2, z)` and `(x+2, y+2, z+4)`

Among those six blocks: exactly one `mekanika:fusion_controller`, exactly one `mekanika:fusion_fuel_port`, and exactly four `mekanika:fusion_laser`. Every other frame block is `mekanika:fusion_casing`, except that one `mekanika:fusion_logic_adapter` may replace a casing that is not one of those six. The interior center `(x+2, y+2, z+2)` is `mekanika:fusion_core`. The other 26 interior blocks are air.

### Tanks and buffers

Deuterium and tritium each have a tank of `fusion.tank_capacity_mb`. The fuel port accepts both chemicals into the matching tank. Its mode is Chemical Input or None. The default is Chemical Input.

The ignition buffer holds at most `fusion.ignition_energy_j`. While idle or charging, the controller's energy mode is Input and it accepts at most `fusion.idle_input_j_per_tick`. While running, the mode switches to Output on its own.

The output buffer holds at most `fusion.output_buffer_j`.

### Tick

States are `idle`, `charging`, `holding`, and `running`.

A tick runs exactly one state branch. The tick that enters `running` does not also emit output.

1. `idle` and `charging` accept energy into the ignition buffer. The state is `charging` once the buffer is greater than 0 and below the ignition cost. The tick that fills the buffer sets the state to `holding` and sets `holdTicks` to 0.
2. Each `holding` tick consumes `fusion.fuel_mb_per_tick` of deuterium and the same of tritium, and increments `holdTicks`. Output energy is 0. If either fuel is short, `holdTicks` resets to 0 and the ignition buffer stays full. When `holdTicks` reaches `fusion.hold_ticks`, set the state to `running`, set the ignition buffer to 0, and play the ignite sound.
3. Each `running` tick consumes 2 mB of each fuel and adds `fusion.output_j_per_tick` to the output buffer. Energy that does not fit in the 2,000,000 J cap is lost. The GUI status is `energy_capped` on a tick that loses energy. Fuel is still consumed. A tick that is short of fuel increments `dryTicks`. A tick with both fuels resets `dryTicks` to 0. At `fusion.quench_ticks` dry ticks, quench.

Quench sets the state to `idle`, sets `dryTicks` and `holdTicks` to 0, leaves the output buffer in place, and sends `fusion_quench`. Breaking any frame block while the state is not `idle` quenches first and emits `fusion.break_radiation` once at the controller, then unforms.

Running radiation strength is `fusion.running_radiation` sieverts per hour at the controller. Idle, charging, and holding emit 0. The break burst is added once and then decays. Dose math is in [RADIATION_SYSTEM.md](RADIATION_SYSTEM.md).

The comparator reads 0 when idle, 8 when charging or holding, and 15 when running.

### Worked start

From empty, at the full idle input of 50,000 J/t, the ignition buffer fills on tick 200 and the state becomes `holding` at the end of that tick. Ticks 201 through 300 are the hold. They consume 200 mB of each fuel and produce no electrical output. At the end of tick 300 the state becomes `running`. Tick 301 is the first tick that adds 1,000,000 J to the output buffer.

### Save

State, ignition buffer, output buffer, hold ticks, dry ticks, both fuel amounts.

### GUI fields

State, ignition progress, deuterium, tritium, output rate, output buffer, radiation strength, laser count. The laser count on a formed reactor is 4.

### Runtime status

`idle`, `charging`, `holding`, `running`, `energy_capped`, `quenched`.

### Config card

Copies the fuel port mode and the redstone mode. It does not copy the automatic energy direction.

## Registry names

Block and chemical names use the localization keys in [LOCALIZATION.md](LOCALIZATION.md).

| Id | English |
| --- | --- |
| `dynamic_tank_casing` | Dynamic Tank Casing |
| `dynamic_tank_glass` | Dynamic Tank Glass |
| `dynamic_tank_valve` | Dynamic Tank Valve |
| `dynamic_tank_controller` | Dynamic Tank Controller |
| `evaporation_casing` | Evaporation Casing |
| `evaporation_valve` | Evaporation Valve |
| `evaporation_controller` | Evaporation Controller |
| `evaporation_solar_panel` | Evaporation Solar Panel |
| `boiler_casing` | Boiler Casing |
| `boiler_glass` | Boiler Glass |
| `boiler_valve` | Boiler Valve |
| `boiler_controller` | Boiler Controller |
| `boiler_logic_adapter` | Boiler Logic Adapter |
| `superheating_element` | Superheating Element |
| `pressure_disperser` | Pressure Disperser |
| `sps_casing` | SPS Casing |
| `sps_port` | SPS Port |
| `sps_controller` | SPS Controller |
| `sps_coil` | SPS Coil |
| `induction_casing` | Induction Casing |
| `induction_glass` | Induction Glass |
| `induction_port` | Induction Port |
| `induction_controller` | Induction Controller |
| `induction_cell_basic` | Basic Induction Cell |
| `induction_cell_advanced` | Advanced Induction Cell |
| `induction_cell_elite` | Elite Induction Cell |
| `induction_cell_ultimate` | Ultimate Induction Cell |
| `induction_provider_basic` | Basic Induction Provider |
| `induction_provider_advanced` | Advanced Induction Provider |
| `induction_provider_elite` | Elite Induction Provider |
| `induction_provider_ultimate` | Ultimate Induction Provider |
| `turbine_casing` | Turbine Casing |
| `turbine_valve` | Turbine Valve |
| `turbine_vent` | Turbine Vent |
| `turbine_controller` | Turbine Controller |
| `turbine_rotor` | Turbine Rotor |
| `turbine_blade` | Turbine Blade |
| `turbine_coil` | Turbine Coil |
| `fission_casing` | Fission Casing |
| `fission_glass` | Fission Glass |
| `fission_port` | Fission Port |
| `fission_controller` | Fission Controller |
| `fission_fuel_assembly` | Fission Fuel Assembly |
| `fission_control_rod` | Fission Control Rod |
| `fission_logic_adapter` | Fission Logic Adapter |
| `fusion_casing` | Fusion Casing |
| `fusion_controller` | Fusion Controller |
| `fusion_fuel_port` | Fusion Fuel Port |
| `fusion_laser` | Fusion Laser |
| `fusion_core` | Fusion Core |
| `fusion_logic_adapter` | Fusion Logic Adapter |
| `brine` (fluid) | Brine |
| `steam` | Steam |
| `lithium` | Lithium |
| `spent_fuel` | Spent Fuel |
| `isomer_gas` | Isomer Gas |
| `fissile_fuel` | Fissile Fuel |
| `deuterium` | Deuterium |
| `tritium` | Tritium |

Block ids in the table use the `mekanika` namespace. `spent_fuel` carries the chemical attribute id `radioactive`. Steam, deuterium, tritium, isomer gas, fissile fuel, and spent fuel are gases. Lithium is liquid-like. The other chemicals do not carry `radioactive`. Brine is a fluid. Water is `minecraft:water`.

## Debug fields

Reported on the formed controller, in addition to the shared multiblock lines in [DEBUG_TOOL_SPEC.md](DEBUG_TOOL_SPEC.md):

- Tank: mode, amount, capacity, dump mode.
- Evaporation: height, efficiency permille, progress, brine, lithium.
- Boiler: display temperature, heat buffer, steam rate, scram flag.
- SPS: coil count, buffer, operation ticks.
- Matrix: stored energy, capacity, max transfer.
- Turbine: limiting status, blade count, coil count, vent count, steam temperature.
- Fission: heat, insertion, scram, radiation strength, burn mB this tick.
- Fusion: state, ignition buffer, output buffer, dry ticks.

## Worked-example index

These are the assertion values. The chapters above show the steps.

```normative-examples
example.tank.3x3x3.capacity_mb=16000
example.tank.5x4x6.capacity_mb=384000
example.evap.h1.panels4.ticks_per_mb=120
example.evap.h16.panels4.after_7_ticks.lithium_mb=0
example.evap.h16.panels4.after_7_ticks.progress=112000
example.evap.h16.panels4.after_8_ticks.lithium_mb=1
example.evap.h16.panels4.after_8_ticks.progress=8000
example.evap.h16.panels2.ticks_per_mb=15
example.boiler.cap1000.boil_heat=167
example.boiler.cap1000.coolant_heat=896
example.boiler.3x3x3.tick5.accept=7
example.boiler.3x3x3.tick5.steam_mb=4
example.boiler.3x3x3.tick5.heat_after=159
example.boiler.3x3x3.tick6.accept=8
example.sps.1coil.ticks=100
example.sps.1coil.energy_left_j=0
example.sps.3coil.ticks=34
example.sps.3coil.energy_left_j=20000
example.sps.5coil.ticks=20
example.sps.5coil.energy_left_j=0
example.matrix.one_basic_cell.capacity_j=1000000000
example.matrix.one_basic_cell.rate=0
example.matrix.18c9p.capacity_j=18000000000
example.matrix.18c9p.rate=144000
example.turbine.5x5x17.steam_mb=3600
example.turbine.5x5x17.energy_j=36000
example.turbine.5x5x17.status=vent-limited
example.turbine.5x5x5.steam_mb=2400
example.turbine.5x5x5.energy_j=24000
example.turbine.5x5x5.status=blade-limited
example.turbine.5x5x5.zero_coils.status=coil-limited
example.fission.one_uncooled.scram_tick=200
example.fission.one_cooled.net_heat=0
example.fusion.charge_ticks=200
example.fusion.hold_fuel_mb_each=200
example.fusion.state_running_on_tick=300
example.fusion.first_output_tick=301
```
