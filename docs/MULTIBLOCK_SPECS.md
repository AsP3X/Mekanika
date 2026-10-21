# Multiblock Validation Sheet

Pass/fail rules, tick order, and assertion values for the eight Mekanika multiblocks. The narrative, GUI field lists, lang lines, and registry names are in [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md). The two normative blocks in this file match that document. Update both files in the same change.

Formation phases are in [MULTIBLOCK_FORMATION.md](MULTIBLOCK_FORMATION.md). The formation period is 10 ticks.

## Shared checks

Run these before the structure-specific postcheck.

1. Flood-fill the six face neighbors, staying on frame blocks of this structure. Interior blocks are not part of the fill.
2. Take the inclusive bounding box of the fill.
3. Fail `chunk_unloaded` when any chunk of that box is unloaded. Log it at debug. Do not send chat. Retry on the next period.
4. Fail `not_cuboid` when a filled block is off the mathematical frame, or when the frame is not exactly the filled set.
5. Fail `frame_hole` on an empty frame position.
6. Fail `edge_block` when an edge is anything other than that structure's casing. Turbine vents, evaporation solar panels, and fusion face centers are not edges. They sit on faces.
7. Fail `missing_controller`, `multiple_controllers`, or `controller_on_edge` unless exactly one controller sits on a face.
8. Fail `already_formed` when a filled block is claimed by another formed structure.
9. Fail `too_small` or `too_large` against the structure maximum after applying `multiblock.max_exterior`.
10. Scan the interior only after the frame passes.
11. `isAir()` passes an air interior check. Fluids and plants fail it. Entities are ignored.
12. Facing is ignored.
13. On success, non-controller blocks remember the controller position and open its GUI. Only the controller and the port or valve blocks are block entities.
14. A controller with stored energy or a non-empty tank breaks only while the player sneaks, and that break voids the stored resources.
15. Port budgets use ascending Y, then Z, then X.
16. Comparator default is `floor(value * 15 / max)`, or 0 when `max` is 0.
17. Save `mekanika:data_version` = 1. Recalculate positions and aggregated rates on load. Refuse a newer version.
18. Sync formation and the last failure immediately. Sync amounts and rates at most every 10 ticks.

Steam stacks store `temperature_deci_c`. A missing value is 1000. Merging uses `(a1 * t1 + a2 * t2) / (a1 + a2)`.

## Dynamic Tank

- Exterior sides 3 through 18, any rectangle inside that limit.
- Edges: `dynamic_tank_casing` only.
- Faces: casing, `dynamic_tank_glass`, `dynamic_tank_valve`, or one `dynamic_tank_controller`.
- Interior: air only.
- `capacityMb = interiorVolume * 16000`.
- First execute insert locks fluid or chemical. Simulate does not lock. Amount 0 clears the lock. The GUI may lock a type early.
- Valve and controller modes: Input, Output, Both, None. Default Both.
- Auto-eject pushes through Output and Both faces up to 4000 mB per tick total.
- Dump Excess deletes amount above the GUI target at 4000 mB per tick. The default target is capacity. Dump All deletes at that same rate.
- Controller GUI has one fill slot and one drain slot.
- Assertions: 3×3×3 holds 16,000 mB. 5×4×6 holds 384,000 mB.

## Thermal Evaporation Plant

- Exterior X and Z are 4. Height is 3 through 18. Otherwise `wrong_fixed_size`.
- `h = height - 2`.
- Bottom layer and all edges: `evaporation_casing`.
- Top inner 2×2: `evaporation_solar_panel`. A gap is `solar_missing`. Top rim stays casing.
- Side faces: casing, `evaporation_valve`, or one `evaporation_controller`.
- Interior 2×2×h: air.
- A panel counts as uncovered when the column above it is air.
- Night (`dayTime % 24000 >= 12000`), thunder, or a dimension without skylight sets efficiency to 0.
- `efficiencyPermille = uncoveredPanels * 250`, then integer-divide by 2 while it is raining and the value is not already 0.
- Each tick add `h * efficiencyPermille` to progress. While progress is at least 120,000 and the recipe can run, subtract 120,000, complete one cycle, and check again. Default numbers complete at most one cycle per tick. If the recipe cannot run, clamp progress down to 120,000.
- Default recipe: 10 mB `mekanika:brine` fluid to 1 mB `mekanika:lithium` chemical. Several matches pick the lowest recipe-id string.
- Input capacity `h * 16000`. Output capacity `h * 8000`.
- New valves use `midY = minY + exteriorHeight / 2` with integer division. Y below `midY` defaults to Brine Input. Y at or above `midY` defaults to Lithium Output.
- The plant forms with missing valves and then reports `missing_valve`.
- Assertions: height interior 1 and four clear panels produce 1 mB every 120 ticks. Interior 16 and four clear panels: after 7 ticks progress is 112,000 and lithium is 0; after 8 ticks lithium is 1 and progress is 8,000. Two panels, or four panels in rain, produce 1 mB every 15 ticks at interior 16.

## Thermoelectric Boiler

- Exterior sides 3 through 18.
- Edges: `boiler_casing`. Faces: casing, `boiler_glass`, `boiler_valve`, or one `boiler_controller`.
- Lowest interior layer: all `superheating_element`. When interior height is at least 2, the highest interior layer is all `pressure_disperser` and the middle is air. Interior height 1 has elements and no dispersers.
- `E = ix * iz`. `D = E` when interior height is at least 2, else 0.
- Water blocks `B` are the air count, or `E` when the air count is 0.
- Water, steam, and sink capacities are `B * 16000` mB.
- `heatCapacity = E * 1000`.
- `displayDeciC = 200 + heatBuffer * 4800 / heatCapacity`.
- Boiling heat is the smallest buffer with display at least 1000. At capacity 1000 that heat is 167.
- Coolant help starts at display 4500. At capacity 1000 that heat is 896.
- `acceptHeat` only queues. The queue refuses heat past `heatCapacity`, past `E * 40` per tick, and, when `D` is 0, past the boiling heat.
- Tick: commit the queue; if scrammed, subtract 100 heat and stop; if boiling, water and steam room allow it, `steamMb = min(E * 4, water, steam room, heatBuffer / 2)`, then spend `steamMb * 2` heat. Steam temperature is 1000 when `D` is 0, otherwise the pre-spend display.
- Then, at display 4500 or more, consume up to 10 mB sink water and remove 50 heat per mB.
- Display at 5000 or more increments `scramTimer`. Otherwise reset it. At 600, scram. The player clears the flag. The structure stays formed.
- `pressureKpa = 100 + steamStored * 400 / steamCapacity + max(0, steamDeciC - 1000) / 10`. This number is display-only.
- Valve modes: Water Input, Steam Output, Coolant Input, None.
- Assertion, 3×3×3, 40 heat offered each tick, water and steam room full: ticks 1–4 accept 40 and make no steam, ending at heat 40, 80, 120, 160. Tick 5 accepts 7, makes 4 mB steam, and ends at heat 159. Tick 6 accepts 8, makes 4 mB, and ends at heat 159.

## Supercritical Phase Shifter

- Every exterior side is 3, 5, or 7.
- Edges and ordinary faces: `sps_casing`. Faces may be `sps_port` or one `sps_controller`. No glass.
- The interior center column is `sps_coil`. Coil count is 1, 3, or 5. Any other arrangement is `coil_placement`. The rest of the interior is air.
- Buffer capacity is `coils * 1,000,000` J. Input cap is `coils * 10,000` J per tick. Each chemical tank holds 64,000 mB.
- Controller energy modes: Input, None. Default Input.
- Port modes: Chemical Input, Chemical Output, None.
- Default recipe: 1,000 mB `mekanika:spent_fuel`, 1,000,000 J, minimum 20 ticks, output 1 mB `mekanika:isomer_gas`. Fuel is consumed on completion. Leftover buffer energy stays. Removing fuel after the start pauses completion and keeps progress.
- Assertions, with the controller offered at least its input cap every tick and both tanks able to run: 1 coil finishes on tick 100 with 0 J left. 3 coils finish on tick 34 with 20,000 J left. 5 coils finish on tick 20 with 0 J left.

## Induction Matrix

- Exterior sides 3 through 18.
- Edges: `induction_casing`. Faces: casing, `induction_glass`, `induction_port`, or one `induction_controller`.
- Every interior block is an induction cell or provider of Basic, Advanced, Elite, or Ultimate tier. Air fails.
- Cell joules: 1e9, 8e9, 64e9, 512e9. Provider joules per tick: 16,000, 64,000, 256,000, 1,024,000.
- Capacity is the cell sum. `maxTransfer` is the provider sum. One shared budget covers input and output in the same tick.
- Energy above a recalculated capacity is deleted and reported.
- Port and controller modes: Input, Output, Both, None. Default Both. Zero providers still form, with transfer 0.
- Assertions: one basic cell stores 1,000,000,000 J at rate 0. Eighteen basic cells and nine basic providers store 18,000,000,000 J at 144,000 J/t.

## Industrial Turbine

- X equals Z. X, Y, and Z are odd and from 5 through 17. Else `not_square` or `wrong_parity`.
- Edges, bottom face, and ordinary side faces: `turbine_casing`.
- Every inner top block is `turbine_vent`. A gap is `vent_missing`.
- Side faces may be `turbine_valve` or one `turbine_controller`.
- The interior center column is `turbine_rotor`. A hole is `rotor_gap`.
- A blade must have a gap-free cardinal path to the rotor in the same layer. Otherwise `blade_gap`.
- Any remaining interior block may be `turbine_coil` or air.
- `bladeFlow = blades * 200`. `ventFlow = vents * 400`. `steamCap = min(bladeFlow, ventFlow)`.
- `joulesPerMb = 10 + max(0, steamDeciC - 1000) / 100`.
- `coilCap = coils * 5000`. `coilSteamCap` is 0 when there are no coils, otherwise `coilCap / joulesPerMb`.
- `actualSteam = min(steamInBuffer, steamCap, coilSteamCap)`. Unconsumed steam stays in the buffer.
- Status, first match: empty buffer is `steam-limited`; `coilSteamCap` of 0, or a coil cap at or below both flow caps that `actualSteam` hits while steam is present, is `coil-limited`; `bladeFlow <= ventFlow` with `actualSteam` equal to `bladeFlow` is `blade-limited`; `actualSteam` equal to `ventFlow` is `vent-limited`; otherwise `steam-limited`.
- Buffers hold 20 ticks of their cap.
- Valves default to steam Input. The controller defaults to energy Output.
- Assertions at 100.0°C with a full steam buffer: a 5×5×17 with 60 blades, 9 vents, and 20 coils moves 3,600 mB and 36,000 J, `vent-limited`. A 5×5×5 with 12 blades, 9 vents, and 12 coils moves 2,400 mB and 24,000 J, `blade-limited`. The same 5×5×5 with 0 coils is `coil-limited` and moves 0 mB.

## Fission Reactor

- Exterior sides 3 through 18.
- Edges: `fission_casing`. Faces: casing, `fission_glass`, `fission_port`, one `fission_controller`, or a top-face `fission_control_rod` on a fuel column's XZ.
- Fuel in one XZ column must be one contiguous Y range. A gap is `fuel_gap`. A column with fuel and no rod above it is `uncontrolled_fuel`. A rod over an empty column is legal.
- `maxHeat = assemblies * 20000`. Each tank holds `max(8000, assemblies * 4000)` mB.
- Insertion 0 withdraws the rods. Insertion 100 inserts them.
- When redstone control is on, `insertion = signal * 100 / 15`.
- When not scrammed and waste room is greater than 0: `requestedMilli = assemblies * (100 - insertion) * 10`, capped by fuel on hand. Accumulate milli-mB and commit whole mB. Cap the committed mB by waste room. Do not cap it by water or steam.
- Each committed mB consumes 1 fuel, emits 1 `spent_fuel`, and adds 100 heat. `cooledMb = min(burnMb, water / 10, steamRoom / 10)` consumes 10 water and emits 10 steam per cooled mB, and removes 100 heat per cooled mB.
- When the committed burn is 0, subtract `max(1, heat / 200)`, floored at 0.
- After those steps, `heat >= maxHeat` with at least one assembly scrams, forces insertion 100, and emits one burst of `assemblies * 1000`. The player clears scram at or below half max heat.
- Zero assemblies after a rebuild clears heat and scram.
- While burning and not scrammed, strength is `assemblies * (100 - insertion)` plus `wasteMb / 1000`.
- Steam energy shown on the GUI uses the turbine joules-per-mB formula. The reactor does not push Joules.
- Port modes: Coolant In, Steam Out, Fuel In, Waste Out, None. All four jobs must exist before a burn. Formation itself does not require them.
- Default fuel file `data/mekanika/fission_fuel/fissile_fuel.json` uses the ratios above.
- Assertions: one assembly, insertion 0, no water, reaches heat 20,000 and scrams on tick 200. The same assembly with 10 mB water and steam room each tick has net heat 0.

## Fusion Reactor

- Exterior 5×5×5 only. A server max below 5 reports `too_large`.
- The six face centers are one `fusion_controller`, one `fusion_fuel_port`, and four `fusion_laser`. Every other frame block is `fusion_casing`.
- Interior center is `fusion_core`. The other 26 interior blocks are air.
- Each fuel tank holds 64,000 mB. Ignition buffer cap is 10,000,000 J. Idle input cap is 50,000 J/t. Output buffer cap is 2,000,000 J.
- A tick runs one state branch.
- Charging fills the ignition buffer. The tick that reaches 10,000,000 J enters `holding` and sets `holdTicks` to 0.
- Each holding tick consumes 2 mB of deuterium and 2 mB of tritium. A short tick resets `holdTicks` and keeps the ignition buffer. The tick that reaches 100 hold ticks enters `running`, clears the ignition buffer, and does not also emit output.
- Each running tick consumes 2 mB of each fuel and adds 1,000,000 J. Energy past the output cap is lost. Twenty consecutive dry ticks quench to `idle` and keep the output buffer.
- Breaking the frame while the state is not `idle` emits break radiation 500, then unforms. Running radiation strength is 2. Other states emit 0.
- Comparator: 0 idle, 8 charging or holding, 15 running.
- Assertion from empty at 50,000 J/t: charge completes on tick 200, the hold consumes 200 mB of each fuel, the state is `running` at the end of tick 300, and tick 301 is the first output tick.

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

## Worked-example index

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
