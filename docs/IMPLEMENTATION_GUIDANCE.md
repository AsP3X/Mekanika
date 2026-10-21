# Implementation Guidance for Mekanika

This document expands on the clean-room analysis with practical, detailed design guidance for high-value systems. All content is conceptual and original. No Mekanism code or assets are referenced.

## 1. Multiblock Invalidation and Reformation Protocol

### Goals
- Detect structural breakage quickly and accurately.
- Clean up formed state without leaking references or capabilities.
- Re-attempt formation efficiently when the structure becomes valid again.
- Avoid thrashing during rapid building, destruction, or lag spikes.

### Recommended Approach
- Each formed structure tracks its set of structural (frame) positions, a bounds volume, and a set of known internal positions.
- On block break, placement, or neighbor update inside the bounds:
  - If a structural/frame block changed → mark the structure fully invalid and schedule reformation.
  - If only an internal component changed state (e.g., activation, not replacement) → update aggregated stats only if needed; skip full revalidation.
- Reformation uses the same request-driven, rate-limited path as initial formation (e.g., at most twice per second).
- On invalidation:
  - Clear controller links and bounding block proxies.
  - Notify internal components that the structure has been dissolved.
  - Release any cached capabilities, networks, or temporary objects.
  - Mark the MultiblockCache as invalid.
- Apply a short cooldown (or skip next few ticks) after failed reformation attempts.
- Fail-fast: if a frame is already known invalid this tick, abort early.

### Edge Cases to Handle
- Shared faces or glass between adjacent multiblocks.
- Partial destruction followed by rapid rebuild.
- Chunk unload/reload while formed (re-validate on chunk load).
- Controller moved or replaced mid-structure.
- Internal blocks replaced with invalid blocks.

### Performance Notes
- Prefer block-state checks over tile entity lookups during invalidation checks.
- Cache chunk references during a validation pass.
- Rate-limit both formation and reformation globally if many structures are dirty.

## 2. Internal Component Aggregation

### Goals
- Collect statistics (capacity, transfer rate, efficiency, required counts, etc.) from mixed-tier internal components at formation time.
- Keep aggregation data-driven, extensible, and cheap.

### Recommended Approach
- Define a simple interface or component for internal blocks that exposes their contribution (capacity, rate, multiplier, role).
- During formation, after shape validation succeeds, iterate internal positions once and sum contributions.
- Store aggregated values in the MultiblockData object (not on every block).
- Support mixed tiers without requiring uniformity.
- Allow components to declare special roles (required minimum count, unique controller, port type).
- Postcheck phase of the validator can enforce minimum counts or specific layouts.

### Examples of Aggregated Stats
- Induction Matrix: sum of cell capacities + sum of provider rates.
- Turbine: blade count, coil count, vent rate.
- Fission: fuel assembly count, control rod positions.

## 3. Controller and Bounding Block Design

### Goals
- One clear owner of the formed structure’s data and GUI.
- Non-controller blocks can still open the GUI and forward interactions.
- Clean teardown on invalidation.

### Recommended Approach
- Designate one block type as the controller, or allow any valid casing to become the controller on formation (prefer the one that initiated formation).
- The controller tile holds the primary MultiblockData reference.
- Other structural blocks store only the controller’s BlockPos (or a weak reference).
- Right-click on any structural block opens the controller’s GUI.
- Bounding/proxy blocks (used for large hitboxes) forward interactions and capabilities to the controller.
- On invalidation, clear all controller references and reset bounding blocks.
- Enforce exactly one controller per structure in the postcheck phase.

## 4. Persistence Strategy

### Goals
- Minimize saved data and NBT size.
- Recalculate what can be cheaply rebuilt on load.
- Avoid desyncs between cache and live data.

### Recommended Approach
- **Save**: controller position, formed flag, key aggregated stats that are expensive to recompute, player-configured settings (side config on ports, security, redstone control), and any non-recalculable state (e.g., current fuel levels in reactors).
- **Recalculate on load**: full set of locations, internal component list, bounds, derived rates.
- Use MultiblockCache to store the minimum required state.
- On world load or chunk load, schedule a validation pass for controllers to re-link structures.
- Avoid saving full position sets for large structures if re-exploration is cheap.
- Rate-limit force-saving of large structures (e.g., only when contents change significantly).

## 5. Side Configuration Model

### Goals
- Consistent per-face configuration for items, fluids, chemicals, energy, and heat.
- Easy to copy, sync, display, and extend.

### Recommended Approach
- Represent side config as a map from RelativeSide to a DataType (None, Input, Output, Energy, Extra, etc.).
- Store one ConfigInfo per TransmissionType on the tile (via a TileComponentConfig-style component).
- Support ejecting flag and canEject per type.
- Machines declare which TransmissionTypes they support; the UI only shows relevant tabs.
- Configurator tool cycles modes on the hovered side for the active transmission type.
- On side change: update capabilities, notify neighbors, and mark dirty.
- Make the entire configuration serializable for config cards and NBT.
- Default configurations can be overridden per face.

### Color Language (example)
- None: grey/black
- Input: dark red
- Output: dark blue
- Energy: green
- Extra/Chemical: purple or cyan

## 6. Chemical Tank and Network Design

### Goals
- Chemicals are first-class resources with clear storage, transfer, and filtering rules.
- Consistent behavior across machines, tanks, and tubes.

### Recommended Approach
- ChemicalStack: type + amount (long). Returned stacks should not be mutated by callers.
- IChemicalTank: capacity, stored, insert, extract, set, onContentsChanged, isValid.
- IChemicalHandler: multi-tank, sided support, insert/extract with Action (execute/simulate).
- Support merged tanks (multiple chemical types, but only one type present at a time).
- Pressurized Tubes form DynamicBufferedNetwork-style networks with capacity and throughput.
- Support push (auto-eject) and pull modes via side config.
- Dump modes for tanks (none, dump excess, dump all).
- Keep chemical networks fully separate from fluid and item networks.
- Document the unit (mB) and any conversion rules early.

## 7. Factory Parallel Processing

### Goals
- Multiple process slots share recipe logic, energy, and chemical inputs efficiently.
- Auto-sort reduces manual management.

### Recommended Approach
- Base factory tile manages an array of process slots (each with input, output, progress).
- Slot count determined by tier (e.g., 3/5/7/9).
- Recipe lookup is shared; each slot tracks its own progress and current input.
- Energy and chemical consumption are summed across active slots each tick.
- Auto-sort moves matching items into empty slots when inventory changes.
- Factories reuse the same recipe handlers and chemical tanks as their single-slot counterparts.
- Support sorting and filtering options in the GUI.

## 8. Energy Network Model

### Goals
- Predictable energy movement with clear capacity vs. throughput separation.
- Works for simple machines and large storage systems.

### Recommended Approach
- Energy containers expose capacity and current amount (long, Joules).
- Transmitters store the buffer in [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md). A cable buffer equals its throughput.
- The network count is for debug. The network does not keep a second copy of the buffer.
- Machines can push energy or allow pull based on side config.
- Large storage (Induction Matrix style) separates storage cells (capacity) from providers (transfer rate).
- Events for energy transfer enable debugging and integration.
- Document the unit and any conversion rules (e.g., to Forge Energy) early.

## 9. Debug / Network Reader Equivalent

### Goals
- Early visibility into structure status, network health, and formation failures.

### Recommended Approach
- Add a lightweight debug item or command early in development.
- On use, display:
  - Whether a multiblock is formed, its size, and controller position.
  - Network transmitter count, capacity, buffer, and throughput.
  - Last formation failure reason and position (if any).
  - Energy or chemical levels on the targeted block.
- Gate the tool behind a config flag or creative-only access during early development.
- Support both item use and a chat command for server-side debugging.

## 10. JEI/EMI Multiblock Preview Approach

### Goals
- Let players see structure requirements and costs before building.

### Recommended Approach
- Define structure requirements in data (min/max size, required blocks, internal components, costs).
- Provide a JEI/EMI category or info page that reads this data.
- Show material counts, a simplified diagram or description, and any special rules.
- Avoid hard-coding layouts; derive counts from the validator rules where possible.
- Include upgrade and tier information where relevant.

## 11. Config Card / Copy-Paste System

### Goals
- Reduce repetitive configuration of machines.

### Recommended Approach
- A config card item that can copy side configuration, upgrades, security settings, and redstone control.
- On paste, apply settings to the target machine if it supports the same transmission types.
- Support batch operations on factories where practical.
- Store the copied data as a simple serializable object (NBT or data component).
- Provide feedback on success or mismatch.

## 12. Phased Implementation Checklist

Recommended order for shared frameworks before individual machines:

1. PowerAPI energy containers, basic transfer, and Fabric implementation + bridges.
2. Chemical stacks, tanks, sided handlers, and basic tube networks.
3. Shared machine base with side configuration, upgrades, redstone, and security components.
4. Multiblock framework (FormationProtocol, validators, managers, data, caches, invalidation).
5. Transmitter networks (energy, chemical, item, heat).
6. Configurator tool.
7. Factory parallel slot system.
8. Debug tools.
9. Individual machines and processing structures.
10. Generators, nuclear, storage (Induction Matrix, QIO), and gear.

## 13. Testing Strategy for Multiblocks

### Goals
- Verify formation, invalidation, and edge cases without full integration tests for every structure.

### Recommended Approach
- Unit-test validators with synthetic block maps and expected FormationResults.
- Test formation and invalidation with small structures in a controlled environment.
- Cover edge cases: rapid placement, max size, mixed-tier internals, shared faces, chunk boundaries, controller conflicts.
- Use the debug tool during manual testing to confirm state.
- Add regression tests when formation or invalidation bugs are fixed.
- Profile large structures early to catch performance regressions.

## 14. Data-Driven Balance Sheet

### Goals
- Allow balance changes without code edits.
- Let design notes feed directly into data.

### Recommended Approach
- Store tier multipliers, energy costs, processing speeds, chemical ratios, and structure limits in config files or data packs.
- Machines and structures read these values at initialization or from data generators.
- Provide a clear mapping from design documents to data keys.
- Use data generation for recipes and tags.
- Document the balance keys so Claude’s notes can be applied directly.

---

These guidelines complement the existing clean-room analysis, formation protocol, and supporting documents. They are intended to reduce rework, improve performance, and provide a clear path for implementation.
