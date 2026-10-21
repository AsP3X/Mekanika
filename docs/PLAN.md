# Mekanika — Full Development Plan

## Goal

Deliver the complete Mekanism experience for Minecraft 26.2 on Forge, Fabric, and NeoForge:

- Osmium-style progression with 4 tiers (Basic / Advanced / Elite / Ultimate)
- Factories with 3 / 5 / 7 / 9 processing slots
- Chemical ore processing up to 5×
- Full transmitter networks (items, fluids, gases, energy, heat)
- Multiblock structures (Dynamic Tank, Thermal Evaporation, Boiler, SPS, Induction Matrix, Turbine, Fission, Fusion)
- Advanced storage including QIO
- Modular gear (MekaSuit, Meka-Tool)

## Modular Architecture

| Module | Role | Dependencies |
|--------|------|--------------|
| PowerAPI | Energy foundation | None |
| Core | Machines, chemicals, transmitters, processing multiblocks, gadgets | PowerAPI |
| Generators | All generators + Turbine / Fission / Fusion | Core + PowerAPI |
| Storage | Cubes, Matrix, full QIO | Core + PowerAPI |
| Tools/Gear | Tools, armor, MekaSuit + modules | Core |
| Additions | Optional flavor | Core |

**Loader strategy**: Architectury multi-loader (common logic + thin platform adapters). PowerAPI is the Fabric foundation.

## Texture Requirements (Claude-owned)

- Exactly 16×16 pixels
- Original designs that match Mekanism industrial style but do **not** copy exact block faces
- Tier color language preserved

## Development Phases

1. **Foundation** — Project setup, PowerAPI skeleton, basic energy cube/cable
2. **Core materials & basic machines** — Ores, alloys, Infuser, Enrichment, Crusher, Smelter, factory skeleton
3. **Chemicals & full processing** — Chemical system + 5× ore chain
4. **Transmitters** — All transmitter types and networks
5. **Multiblock framework** — Structure system + Dynamic Tank, Evap, Boiler, SPS
6. **Generators** — Simple generators + Turbine, Fission, Fusion
7. **Storage** — Induction Matrix + full QIO
8. **Tools/Gear** — MekaSuit, Meka-Tool, modules, gadgets
9. **Polish** — JEI/EMI, configs, final balance, testing across loaders

## Roles

- **Grok**: All Java code, Gradle, networking, BlockEntities, registries, platform adapters
- **Claude**: All 16×16 textures, design docs, recipe trees, multiblock blueprints, balance sheets, GUI layouts, supporting documentation

See `AGENTS.md` for binding rules.
