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

Each phase is done when the matching steps in [FRAMEWORK_IMPLEMENTATION_ORDER.md](FRAMEWORK_IMPLEMENTATION_ORDER.md) pass.

1. **Foundation** — Steps 1 through 4. A basic energy cube and a basic cable exist. One enrichment chamber draws 1,000 J/t.
2. **Core materials and basic machines** — Step 9's enrichment, crusher, smelter, and infuser, plus velorite and the three alloys. A 2× ore path yields 2 ingots and costs 600,000 J.
3. **Chemicals and full processing** — The rest of step 9. A 5× ore path yields 5 ingots and costs 12,440,000 J.
4. **Transmitters** — Step 6. Item, fluid, chemical, energy, and heat networks move on the documented costs.
5. **Multiblock framework** — Step 5, then the tank, evaporation plant, boiler, and SPS from step 11.
6. **Generators** — Step 10, then the turbine, fission reactor, and fusion reactor from step 11.
7. **Storage** — Step 12. Matrix cells and a 12-slot QIO array.
8. **Tools and gear** — Step 13.
9. **Polish** — Step 14, on Forge, Fabric, and NeoForge.

## Roles

- **Grok**: All Java code, Gradle, networking, BlockEntities, registries, platform adapters
- **Claude**: All 16×16 textures, the GUI frame and widget atlas, design docs, recipe trees, multiblock blueprints, balance sheets, supporting documentation

See `AGENTS.md` for binding rules.
