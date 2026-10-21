# Framework Implementation Order

Build in this order. A step is done when its checks pass. The checks are the worked-example indexes named here. Do not start a later step's content blocks before the earlier step's checks pass. Platform modules stay thin: registration and energy adapters only.

## Normative constants

```normative
order.steps=14
order.formation_period_ticks=10
order.max_exterior=18
```

## Steps

1. **PowerAPI.** `IEnergyContainer` from [PUBLIC_API_SUMMARY.md](PUBLIC_API_SUMMARY.md). Done when a simulated insert leaves the container unchanged and 1,000 J formats as `1 kJ`.
2. **Chemicals.** Stacks, tanks, and handlers. Done when missing steam temperature reads 1,000 decidegrees and a mismatched insert accepts 0.
3. **Fluids.** The loader fluid handler plus `IFluidView` in millibuckets. Done when one bucket reads 1,000 mB.
4. **Machine base.** Sides, redstone, security, and the eight upgrade slots from [MACHINE_IO.md](MACHINE_IO.md). Done when eight speed upgrades take a 200-tick enrichment batch to 22 ticks at 9,000 J/t, and when one held redstone pulse completes one enrichment batch. The modes are in [CONTROL_SYSTEM.md](CONTROL_SYSTEM.md).
5. **Multiblock framework.** Formation, the 10-tick period, and invalidation that keeps controller contents. Done when a 3×3×3 dynamic tank holds 16,000 mB and a broken frame clears formed state without deleting that amount.
6. **Transmitters.** Energy and heat first, then chemicals, fluids, and items. Done when a 1,000 J cable link syncs a 10 J change immediately and waits on a 9 J change, when a full basic cable beside an empty ultimate cable moves 992 J and the next tick moves 0, when a basic fluid pipe accepts 250 mB from a basic fluid tank in one tick, and when one basic transporter block takes 20 ticks to cross.
7. **Factories.** Slot counts 3, 5, 7, and 9. Done when three basic enrichment slots draw 3,000 J/t.
8. **Configurator and debug.** Face cycling and `/mekanika debug`. Done when a survival player is denied while `general.debug_mode` is false and a creative player is not.
9. **Processing machines.** The twelve machines and the 2× through 5× counts. Done when one velorite ore's 5× path is 12,440,000 J and five ingots.
10. **Early generators and heat.** Done when one coal in a basic heat generator yields 1,600,000 J and 64,000 heat points, and when the boiler 3×3×3 trace reaches 4 mB of steam on tick 5.
11. **Structures.** Dynamic tank, evaporation, boiler, SPS, matrix, turbine, fission, and fusion, using [MULTIBLOCK_SPECS.md](MULTIBLOCK_SPECS.md). Done when every key in that file's worked-example index passes.
12. **Storage.** Induction matrix tiers and QIO. Done when twelve basic drives hold 192,000 items and one basic cell holds 1,000,000,000 J.
13. **Gear and gadgets.** Done when a level-2 radiation module reports resistance 1.0 and four lined pieces report 0.8.
14. **Polish.** Language lines, sounds, JEI, and the loader matrix. Done when the English inventory in [LOCALIZATION.md](LOCALIZATION.md) has a line for every processing machine, the sound table has 9 one-shot events and 5 loops, and the lookup in [JEI_EMI_INTEGRATION.md](JEI_EMI_INTEGRATION.md) registers 12 machine categories and 8 structure pages.

## Worked-example index

```normative-examples
example.steps=14
example.factory3.joules_per_tick=3000
example.coal.energy_j=1600000
example.coal.heat=64000
```
