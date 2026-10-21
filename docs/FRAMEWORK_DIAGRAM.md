# Mekanika Framework Architecture Diagram

This diagram shows the high-level module structure and dependencies for Mekanika.

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

    subgraph Support["Supporting Systems"]
        Debug[Debug Tool]
        GUI[GUI Patterns]
        Sync[Sync Strategy]
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

    MachineBase --> SideConfig
    MachineBase --> Chemicals
    MachineBase --> Fluids
    MachineBase --> Upgrades
    MachineBase --> Control

    Networks --> Chemicals
    Networks --> Fluids
    Networks --> EnergyCore

    Multiblocks --> MachineBase
    Multiblocks --> Networks

    Generators --> Multiblocks
    Storage --> Multiblocks
    Storage --> Networks

    Tools --> EnergyCore

    Core --> Debug
    Core --> GUI
    Core --> Sync

    style PowerAPI fill:#2d5a27,color:#fff
    style Core fill:#1a3a5c,color:#fff
    style Additions fill:#5c3a1a,color:#fff
    style Loaders fill:#3a3a3a,color:#fff
```

## Module Dependency Summary

| Module | Depends On | Provides |
|--------|------------|----------|
| **PowerAPI** | — | Energy unit, containers, handlers, Fabric implementation |
| **Core** | PowerAPI | Machines, chemicals, fluids, networks, multiblocks, side config, control |
| **Generators** | Core + PowerAPI | Power generation, reactors |
| **Storage** | Core + PowerAPI | Energy storage, Induction Matrix, QIO |
| **Tools/Gear** | Core + PowerAPI | Modular gear, tools |

## Key Rules

- Common code lives in the shared module.
- Platform modules (Forge/Fabric/NeoForge) are thin adapters.
- Generators, Storage, and Tools depend only on Core + PowerAPI (no circular dependencies).
- PowerAPI is fully custom on Fabric; bridges or shared implementation on Forge/NeoForge.

See also: [PLAN.md](PLAN.md), [FRAMEWORK_IMPLEMENTATION_ORDER.md](FRAMEWORK_IMPLEMENTATION_ORDER.md), and the [documentation index](README.md).
