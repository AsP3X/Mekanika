# Clean-Room Analysis: Multiblock Formation Protocol

This document analyzes the high-level design of Mekanism’s multiblock formation system based on public package structure, class names, commit messages, and observed gameplay behavior. No code is reproduced.

## Core Components

The multiblock library (`lib/multiblock`) contains the following key pieces:

- **FormationProtocol** — Orchestrates the formation process for a given structure type.
- **MultiblockManager** — One manager per multiblock type. Tracks formed structures and coordinates updates.
- **IStructureValidator** / **CuboidStructureValidator** — Validates that a candidate structure matches the expected shape (typically a rectangular prism) and block requirements.
- **MultiblockData** — Holds the live state of a formed structure (locations, internal components, bounds, capabilities).
- **MultiblockCache** — Persists relevant state across formation/disassembly cycles.
- **Structure** — Represents a candidate or formed structure, including its nodes and controller.
- **Interfaces**:
  - `IMultiblock` / `IMultiblockBase` — Core multiblock tile contract.
  - `IStructuralMultiblock` — Structural (frame) blocks.
  - `IInternalMultiblock` — Internal functional components.
  - `IValveHandler` — Valve/port handling.

## Formation Flow (High-Level)

From commit history and class responsibilities, formation appears to follow these phases:

1. **Trigger**
   - Formation is typically initiated when a structural block is placed, broken, or when a previously invalid structure is re-validated.
   - Updates are request-based rather than running on every tick to reduce load during rapid placement.

2. **Exploration / Shape Detection**
   - Starting from a seed position, the system explores adjacent structural blocks.
   - It collects a set of locations that form a candidate structure.
   - Early optimizations use block checks instead of full tile lookups where possible and fail fast if a frame is known to be invalid this tick.

3. **Validation**
   - A structure-specific validator (commonly a cuboid validator) checks:
     - Dimensions fall within min/max bounds.
     - Edges and faces meet casing requirements.
     - Required internal components are present (e.g., at least one cell and one provider for Induction Matrix).
     - No invalid blocks occupy required positions.
   - Validation can return early failure results without fully assembling data.

4. **Structure Building**
   - If validation succeeds, a `MultiblockData` instance is populated with:
     - Full set of locations.
     - Internal component positions and types.
     - Bounds and relative positioning helpers.
     - Aggregated statistics (capacity, transfer rate, etc.) derived from internal components.

5. **Formation / Activation**
   - Nodes are linked to the formed structure.
   - Internal components receive an `onCreated` (or equivalent) callback.
   - Visual feedback is emitted (e.g., redstone particles).
   - The structure becomes interactive (GUI appears on right-click).

6. **Update & Invalidation**
   - Formed structures listen for block changes inside their bounds.
   - If a structural block is broken or an invalid change occurs, the structure is invalidated and may attempt reformation.
   - Caching is used so that reformation does not require full re-exploration when only internal state changes.

## Key Design Patterns

- **Request-based updates**: Formation is not attempted every tick. Changes mark structures for update, and validation is rate-limited.
- **Fail-fast validation**: Invalid frames or known-bad structures abort early in the same tick.
- **Separation of concerns**:
  - Shape validation (validator)
  - Data aggregation (MultiblockData)
  - Lifecycle management (Manager)
  - Persistence (Cache)
- **Internal vs. structural distinction**: Frame blocks define the bounds; internal components define capacity and behavior.
- **Controller pattern**: One tile typically acts as the controller that owns the MultiblockData and exposes the GUI.

## Observed Gameplay Behavior

- Correct structures form with a particle effect and become interactive.
- Rapid placement of the final blocks can sometimes delay formation until a block is broken and replaced (historical issue related to update timing).
- Large structures (up to 18×18×18) form efficiently after optimizations that reduced formation time from ~100–150 ms to a few milliseconds.
- Internal component tiers can be mixed; the structure aggregates their contributions.

## Learnings for Mekanika

1. Build a reusable formation protocol that is request-driven rather than tick-driven.
2. Separate shape validation from data aggregation and lifecycle management.
3. Support fail-fast checks and rate limiting to handle rapid building.
4. Distinguish structural (frame) blocks from internal functional components.
5. Provide clear visual and GUI feedback on successful formation.
6. Cache formed structure data so minor internal changes do not require full revalidation.
7. Allow internal components to contribute statistics (capacity, rate, etc.) that are aggregated at formation time.
8. Design for mixed-tier internals where appropriate.

This analysis remains strictly clean-room. All Mekanika implementations should be original.
