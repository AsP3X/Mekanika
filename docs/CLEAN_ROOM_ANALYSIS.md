# Clean-Room Analysis: Detailed Systems and Architectural Patterns from Mekanism

This is a conceptual, high-level analysis based solely on public documentation, wiki descriptions, observed gameplay behavior, and structural review of the public source repository (package and class names only). **No code, textures, models, recipes, or proprietary assets are referenced or reproduced.** The goal is to extract proven design patterns so Mekanika can reimplement equivalent systems originally and efficiently.

## 1. Overall Architecture and Modularity

Mekanism is organized as a core mod with official addons (Generators, Tools, Additions). Source is split into dedicated sets: `main`, `generators`, `tools`, `additions`, plus a public `api` package, `datagen`, and tests.

**Key structural observations**:
- Public API (`mekanism.api`) exposes energy, chemical, heat, gear, radiation, recipes, security, and sided handlers.
- Tile entities are heavily organized by concern: `base`, `component`, `machine`, `factory`, `multiblock`, `transmitter`, `qio`, `laser`, `interfaces`, `prefab`.
- Content is further grouped under `content/` (assemblicator, boiler, evaporation, matrix, miner, qio, sps, tank, transporter, etc.).
- Attachments system for modular data (side config, upgrades, ejector, frequency, filters).
- Dedicated network and multiblock libraries under `lib/`.

**Learnings for Mekanika**:
- Thin dependency graph: PowerAPI → Core → Generators / Storage / Tools.
- Component + attachment style keeps base tiles clean and extensible.
- Public API surface for energy, chemicals, and gear enables clean addon development.
- Separate source sets for addons reduce coupling.

## 2. Progression and Tier System

Four tiers (Basic / Advanced / Elite / Ultimate) with consistent color language. Progression is alloy- and circuit-driven. Factories scale via parallel processing slots rather than pure speed.

**Architectural notes**:
- Tier abstractions exist in the API.
- Factory variants live in their own package while sharing recipe logic with base machines.
- In-place upgrade via installers is a first-class player action (supported by advancements).

**Learnings**:
- Parallel slots (3/5/7/9) are more automation-friendly than raw speed multipliers.
- In-place upgrades reduce player friction.
- Data-driven tier progression allows balance changes without code edits.

## 3. Energy System

Universal Joules with conversion support. Energy Cubes act as portable batteries. Networks form dynamically.

**Source patterns**:
- Dedicated energy containers and handlers in both API and attachment systems.
- Component-backed energy containers for tiles and items.
- Clear separation of capacity vs. transfer rate (later exploited by Induction Matrix providers).

**Learnings**:
- Document the energy unit and transfer model early.
- Side configuration for energy is expected.
- Capacity vs. rate separation is a powerful pattern for late-game storage.

## 4. Chemical System

Unified chemical framework covering gases, infuse types, slurries, and pigments. Full tank, stack, handler, and attribute system.

**Architectural depth**:
- Public API has extensive chemical package: `Chemical`, `ChemicalStack`, `IChemicalTank`, `IChemicalHandler`, attributes, validators.
- Attachment-based chemical tanks with builders and component-backed implementations.
- Merged tanks for dual-purpose containers.
- Dedicated chemical production and processing recipes (infusion, dissolution, crystallization, electrolysis, etc.).

**Learnings**:
- Treat chemicals as first-class resources with their own tanks and transmitters.
- Attribute system allows radiation, flammability, and other behaviors to be data-driven.
- Self-sustaining production loops create natural mid-game goals.

## 5. Machines, Factories, and Component System

Machines share a common base (`TileEntityMekanism`) with modular components.

**Component package contents** (from structure):
- `TileComponentConfig` — side configuration
- `TileComponentUpgrade` — upgrade handling
- `TileComponentSecurity` — ownership and access
- `TileComponentEjector` — auto-eject
- `TileComponentChunkLoader` — chunk loading support

Attachments further modularize side config, upgrades, and ejector state.

Factories are separate tile variants that reuse the same recipe logic but process multiple slots in parallel.

**Learnings**:
- Component or attachment system is preferable to deep inheritance.
- Shared recipe logic between base machines and factories is essential.
- Side config + configurator tool is high-value UX.

## 6. Transmitters and Networks

Dedicated transmitter package and network library.

**Network library structure**:
- `DynamicNetwork` / `DynamicBufferedNetwork`
- `TransmitterNetworkRegistry`
- `TransmissionType`, `ConnectionType`
- Compatibility validators and acceptor subsystems

Five primary transmitter types (items with sorting, fluids, gases, energy, heat). Networks support push/pull, buffers, and throughput limits.

**Learnings**:
- Per-resource networks keep logic simpler.
- Registry + dynamic network pattern scales well.
- Early debug/network reader tool is important.

## 7. Multiblock Framework

Reusable multiblock infrastructure under `lib/multiblock`.

**Core pieces**:
- `MultiblockManager` (one per structure type)
- `FormationProtocol` for assembly
- `CuboidStructureValidator` and `IStructureValidator`
- `MultiblockData` for live state
- `MultiblockCache` for persistence
- Interfaces: `IMultiblock`, `IStructuralMultiblock`, `IInternalMultiblock`, `IValveHandler`

Structures are rectangular prisms with configurable min/max sizes. Internal components (cells, providers, fuel assemblies, rotors, etc.) control capacity and rates. Formation provides visual feedback.

**Supported structures** (from content packages and managers): Dynamic Tank, Evaporation, Boiler, SPS, Induction Matrix, Turbine, Fission, Fusion.

**Learnings**:
- A single reusable framework with per-type managers and validators saves massive effort.
- Separate frame validation from internal component stats.
- Caching and update protocols are critical for large structures.
- Clear formation feedback (particles + GUI) is expected.

## 8. Storage Systems

- Tiered bins, energy cubes, fluid/chemical tanks
- Dynamic Tank (flexible multiblock)
- Induction Matrix (cells = capacity, providers = transfer rate)
- QIO (frequency-based wireless item storage)

**QIO structure** (from packages):
- Dedicated `qio` tile and content packages
- Drive arrays, dashboards, importers/exporters
- Frequency and security integration

**Learnings**:
- Capacity vs. transfer rate separation is powerful.
- Frequency-based systems reduce late-game cable clutter.
- Security modes and drive type limits are proven patterns.

## 9. Generators and Nuclear Systems

Simple generators progress to complex multiblock reactors. Fuel cycles are multi-step.

**Patterns**:
- Heat as a transferable resource (Thermodynamic Conductors).
- Clear failure modes and player feedback for fission/fusion.
- Supporting infrastructure (turbine, boiler, evaporation) is modular.

**Learnings**:
- Define failure states and player-visible feedback early.
- Fuel production should be its own process chain.

## 10. Ore Processing Chains

Staged 2× → 5× multiplication. Higher tiers introduce chemical dependencies and reuse lower-tier machines.

**Learnings**:
- Staged progression teaches systems gradually.
- Intermediate forms (clumps, shards, crystals, slurry) make the process visible.
- Chemical production loops are natural mid-game content.

## 11. Gear and Modular Equipment

MekaSuit and Meka-Tool use installable modules. Shared energy system with machines.

**API notes**: Dedicated gear package with module support.

**Learnings**:
- Modular units are more interesting than fixed tiers.
- Shared energy creates cohesion.
- Radiation protection modules are expected if radiation exists.

## 12. Supporting Systems

- **Attachments**: Modular data for tiles and items (side config, upgrades, frequency, filters, overflow).
- **Advancements**: Custom triggers for upgrades, configuration, miner use, etc.
- **Datagen**: Extensive recipe and tag data generation.
- **Security**: Ownership and access modes on machines and storage.
- **Config cards**: Copy/paste machine configuration.

## Summary of Highest-Value Patterns

1. Component/attachment system for tiles (config, upgrades, security, ejector).
2. Public API for energy, chemicals, recipes, and gear.
3. Reusable multiblock framework (manager + validator + formation + data/cache).
4. Dynamic network system for transmitters.
5. Parallel-slot factories sharing recipe logic.
6. Capacity vs. transfer rate separation.
7. Frequency-based wireless storage (QIO).
8. Staged chemical ore processing.
9. Modular gear with shared energy.
10. Data-driven recipes and configs.
11. Early debug tools for networks and multiblocks.
12. Clear player feedback on complex system failures.

All of the above can be reimplemented originally in Mekanika. No code or assets need to be copied. This document should serve as a living reference during design and implementation phases.
