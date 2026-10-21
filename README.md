# Mekanika

**Full Mekanism-like industrial technology mod suite for Minecraft 26.2**

Supports Forge, Fabric, and NeoForge via Architectury multi-loader architecture.

## Modules

- **PowerAPI** — Custom energy system (required foundation, especially for Fabric)
- **Core** — Machines, factories, chemicals, transmitters, processing multiblocks, gadgets
- **Generators** — Power generation including Turbine, Fission, and Fusion reactors
- **Storage** — Energy cubes, Induction Matrix, full QIO system
- **Tools/Gear** — Tiered tools, MekaSuit, Meka-Tool with modules
- **Additions** (optional) — Flavor content

## Architecture

```mermaid
graph TB
    subgraph Loaders["Multi-Loader (Architectury)"]
        Forge[Forge]
        Fabric[Fabric]
        NeoForge[NeoForge]
    end

    subgraph PowerAPI["PowerAPI"]
        EnergyCore[Energy Containers & Handlers]
        FabricEnergy[Fabric Native Implementation]
        Bridges[Forge / NeoForge Bridges]
    end

    subgraph Core["Core Module"]
        MachineBase[Shared Machine Base]
        SideConfig[Side Configuration]
        Chemicals[Chemical System]
        Fluids[Fluid System]
        Networks[Transmitter Networks]
        Multiblocks[Multiblock Framework]
        Control[Control System<br/>Redstone / Security]
        Upgrades[Upgrade System]
        Recipes[Recipe & Data Gen]
    end

    subgraph Additions["Addition Modules"]
        Generators[Generators<br/>Power Sources / Reactors]
        Storage[Storage<br/>Energy Cubes / Matrix / QIO]
        Tools[Tools & Gear<br/>Modules / Suits]
    end

    Loaders --> PowerAPI
    Loaders --> Core

    PowerAPI --> Core
    PowerAPI --> Generators
    PowerAPI --> Storage
    PowerAPI --> Tools

    Core --> Generators
    Core --> Storage
    Core --> Tools

    style PowerAPI fill:#2d5a27,color:#fff
    style Core fill:#1a3a5c,color:#fff
    style Additions fill:#5c3a1a,color:#fff
    style Loaders fill:#3a3a3a,color:#fff
```

See [docs/FRAMEWORK_DIAGRAM.md](docs/FRAMEWORK_DIAGRAM.md) for the full diagram and dependency details.

## Status

Planning & foundation stage. See `docs/PLAN.md` for the complete feature and development plan.

## Agents

- **Grok**: All coding and implementation
- **Claude**: Design, 16×16 original textures, balance, documentation, supporting specs

See `AGENTS.md` for binding rules both agents must follow.

## Documentation

Full design documentation is available in the [`docs/`](docs/README.md) folder.

## License

To be determined by the owner. All work must respect existing project licensing practices.
