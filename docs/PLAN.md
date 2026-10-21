# Mekanika – Full Plan

Minecraft 26.2 | Forge / Fabric / NeoForge | Architectury multi-loader

## Modular Architecture

1. **PowerAPI** (base dependency)
   - Custom Joules energy system.
   - Full implementation on Fabric.
   - Bridges or shared implementation on Forge/NeoForge.
   - Energy storage interfaces, transfer, optional cables.

2. **Core** (main mod)
   - Materials, alloys, circuits, HDPE.
   - All processing machines + factories (3/5/7/9 slots).
   - Full chemical system (gases, fluids, infuse, slurries).
   - Transmitters (items, fluids, gases, energy, heat).
   - Processing multiblocks: Dynamic Tank, Thermal Evaporation, Thermoelectric Boiler, SPS.
   - Digital Miner, Quantum Entangloporter, Robit, Configurator, upgrades, side config, security.
   - Basic storage (bins, tanks).

3. **Generators** (addon)
   - Heat, Solar, Advanced Solar, Wind, Bio, Gas-Burning.
   - Multiblocks: Industrial Turbine, Fission Reactor, Fusion Reactor.

4. **Storage** (addon)
   - Tiered Energy Cubes, tanks, bins.
   - Induction Matrix.
   - Full QIO system.

5. **Tools/Gear** (addon)
   - Material tools/armor, Paxels.
   - MekaSuit + Meka-Tool with full modular system.
   - Jetpack, Atomic Disassembler, Free Runners, etc.

6. **Additions** (optional)
   - Balloons, baby mobs, plastic blocks.

## Feature Coverage

Full Mekanism parity: Osmium-style progression, 5× ore processing, factories, transmitters, all listed multiblocks, QIO, reactors, modular gear, radiation, JEI/EMI integration, configurable values.

## Development Phases

1. Foundation: Architectury setup + PowerAPI skeleton.
2. Core materials & basic machines.
3. Chemicals & full processing chain.
4. Transmitters & logistics.
5. Multiblock framework + processing multiblocks.
6. Generators + reactors.
7. Storage (Induction Matrix + QIO).
8. Tools/Gear + modules.
9. Polish: JEI, configs, balance, testing on all loaders.

Claude owns all 16×16 textures and design support. Grok owns all code.

## Technical Notes

- Architectury for Fabric + NeoForge (Forge support where feasible).
- Common module for majority of logic.
- Data-driven where practical.
- Efficient networking for transmitters and QIO.
- Clear failure modes for nuclear systems.
