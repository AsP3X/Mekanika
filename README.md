# Mekanika

Full Mekanism-like tech mod suite for Minecraft 26.2.

Supports **Forge**, **Fabric**, and **NeoForge** via Architectury multi-loader.

## Modules

| Module | Description |
|--------|-------------|
| **PowerAPI** | Custom energy system (Joules). Full implementation on Fabric; bridges on Forge/NeoForge. |
| **Core** | Main mod: machines, factories, chemicals, transmitters, processing multiblocks, Digital Miner, gadgets. |
| **Generators** | All generators + Industrial Turbine, Fission Reactor, Fusion Reactor. |
| **Storage** | Energy Cubes, Induction Matrix, full QIO system, advanced tanks. |
| **Tools/Gear** | Tiered tools/armor, MekaSuit, Meka-Tool with modules, Jetpack, Atomic Disassembler. |
| **Additions** (optional) | Balloons, baby mobs, plastic, aesthetics. |

## Key Requirements

- All block textures created by Claude: **exactly 16×16 pixels**, Mekanism-inspired style but **original designs**.
- Tier colors: Basic = green, Advanced = red, Elite = cyan, Ultimate = purple.
- Full Mekanism experience: 5× ore processing, tiered progression, multiblocks, QIO, reactors, modular gear.

## Documentation

- [Full Plan](docs/PLAN.md)
- [Agent Rules](AGENTS.md)
- [Texture Handoff Checklist](docs/TEXTURE_HANDOFF.md)

## Development Roles

- **Grok**: All code, Gradle/Architectury setup, systems implementation.
- **Claude**: All 16×16 textures, design docs, recipes, balance, GUI layouts, support.

See `AGENTS.md` for binding rules.
