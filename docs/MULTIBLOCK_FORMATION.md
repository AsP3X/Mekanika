# Multiblock Formation Protocol — Clean-Room Analysis

High-level analysis of Mekanism’s multiblock formation system based on public source structure, class names, commit history, and observed behavior. No code or assets are reproduced.

## Core Components

| Component | Role |
|-----------|------|
| **FormationProtocol** | Coordinates the assembly process. Tracks locations, inner nodes, valves, and unique IDs during formation. Interacts with validators and produces a FormationResult. |
| **MultiblockManager** | Per-structure-type manager. Creates validators, owns caches, and handles lifecycle (formation, updates, destruction). |
| **CuboidStructureValidator** | Default validator for rectangular prism structures. Enforces min/max dimensions, validates frame (edges/faces) vs. inner volume. |
| **IStructureValidator** | Contract for all validators. Supports precheck, frame validation, inner validation, and postcheck phases. |
| **Structure** | Holds the formed state: set of positions, bounds, inner nodes, and associated data. |
| **MultiblockData** | Runtime data for a formed multiblock (inventories, energy, chemicals, formed flag, bounds). |
| **MultiblockCache** | Persistent cache of structure data for performance and re-formation. |
| **IMultiblock / IStructuralMultiblock** | Interfaces distinguishing controllers from structural blocks. |

## High-Level Formation Flow

1. **Trigger**  
   A block placement, break, or tick on a potential controller or structural block initiates an update.

2. **Protocol Start**  
   FormationProtocol is created or reused for the structure. It begins collecting candidate positions.

3. **Validation Phases** (via validator)
   - **Precheck**: Quick size and basic shape verification (e.g., minimum cuboid).
   - **Frame Validation**: Edges and faces must be valid casing/structural blocks. Controllers and ports are allowed in specific positions.
   - **Inner Validation**: Interior positions must be valid internal components (air, cells, providers, fuel assemblies, etc.) or empty as required.
   - **Postcheck**: Structure-specific logic (e.g., exactly one controller, required minimum components, odd dimensions for turbines, no conflicting controllers).

4. **Result**  
   Validator returns a FormationResult (success or failure with reason and optional position).

5. **Activation**  
   On success:
   - Structure is marked formed.
   - MultiblockData is populated.
   - Bounding blocks / proxies are set up so interactions route to the controller.
   - Particles and GUI become available.
   - Cache is updated.

6. **Ongoing Management**  
   - Managers track formed structures.
   - Changes to internal blocks or frame can trigger revalidation.
   - Invalid structures are reset and caches cleaned.

## Specialized Validators

Most multiblocks use CuboidStructureValidator or a thin subclass. Complex ones (Fission, Turbine, Fusion) override postcheck to enforce additional rules:
- Specific internal component counts or positions.
- Odd dimensions for rotational symmetry.
- Unique controller placement.
- Compatibility of internal blocks with the structure type.

## Performance Considerations

- Formation attempts are rate-limited.
- Chunk lookups are cached during validation.
- Structure merging logic avoids redundant full scans when adjacent structures combine.
- Reduced update tags minimize network traffic for large structures.

## Key Design Lessons for Mekanika

- Separate formation protocol from structure-specific validation.
- Use phased validation (precheck → frame → inner → postcheck) for clear failure reporting.
- Keep a reusable cuboid validator as the base; specialize only where needed.
- Controllers should be distinct from pure structural blocks, with bounding blocks proxying interactions.
- Provide clear FormationResult feedback (success/failure + reason + position) for player UX and debugging.
- Cache formed structures and use rate limiting to protect server performance.
- Support structure merging and revalidation without full rebuilds when possible.

This pattern allows a single framework to support Dynamic Tanks, Induction Matrices, Turbines, Reactors, and future structures with minimal duplication.

Per-structure dimensions, blocks, and tick rules are in [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md). The assertion values are in [MULTIBLOCK_SPECS.md](MULTIBLOCK_SPECS.md).
