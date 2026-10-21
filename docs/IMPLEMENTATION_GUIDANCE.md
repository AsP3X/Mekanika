# Implementation Guidance for Mekanika

This document expands on the clean-room analysis with practical design guidance for high-value systems. All content is conceptual and original. No Mekanism code or assets are referenced.

## 1. Multiblock Invalidation and Reformation Protocol

### Goals
- Detect structural breakage quickly.
- Clean up formed state without leaking references.
- Re-attempt formation efficiently when the structure becomes valid again.
- Avoid thrashing during rapid building or destruction.

### Recommended Approach
- Each formed structure tracks its set of structural (frame) positions and a bounds volume.
- On block break or neighbor update inside the bounds:
  - If a structural block changed, mark the structure invalid and schedule a reformation attempt.
  - If only an internal component changed state, update aggregated stats without full revalidation.
- Reformation uses the same request-driven, rate-limited path as initial formation.
- On invalidation:
  - Clear controller links.
  - Notify internal components that the structure has been dissolved.
  - Release any cached capabilities or networks.
- Apply a short cooldown after failed reformation attempts to prevent repeated work.

### Edge Cases
- Shared faces between adjacent multiblocks.
- Partial destruction and rapid rebuild.
- Chunk unload while formed.

## 2. Internal Component Aggregation

### Goals
- Collect statistics (capacity, transfer rate, efficiency, etc.) from mixed-tier internal components at formation time.
- Keep aggregation data-driven and extensible.

### Recommended Approach
- Define a simple interface for internal components that exposes their contribution (e.g., capacity, rate, multiplier).
- During formation, after shape validation succeeds, iterate internal positions once and sum contributions.
- Store aggregated values in the MultiblockData object.
- Support mixed tiers; do not require uniformity.
- Allow components to declare special roles (controller, port, required minimum count).

## 3. Controller and Bounding Block Design

### Goals
- One clear owner of the formed structure’s data and GUI.
- Non-controller blocks can still open the GUI and forward interactions.

### Recommended Approach
- Designate one block type as the controller (or allow any casing to become the controller on formation).
- The controller tile holds the MultiblockData reference.
- Other structural blocks store a reference (or position) to the controller.
- Right-click on any structural block opens the controller’s GUI.
- On invalidation, clear all controller references.
- For large structures, avoid storing full data copies on every block; store only the controller position.

## 4. Persistence Strategy

### Goals
- Minimize saved data.
- Recalculate what can be cheaply rebuilt on load.

### Recommended Approach
- Save: controller position, formed flag, aggregated stats that are expensive to recompute, and any player-configured settings (side config on ports, security).
- Recalculate on load: full set of locations, internal component list, bounds.
- Use the MultiblockCache to store the minimum required state.
- On world load, schedule a validation pass for all controllers to re-link structures.
- Avoid saving full position sets for large structures if they can be re-explored quickly.

## 5. Side Configuration Model

### Goals
- Consistent per-face configuration for items, fluids, chemicals, energy, and heat.
- Easy to copy, sync, and display.

### Recommended Approach
- Represent side config as a map from face to a small set of modes (none, input, output, energy, extra).
- Store the configuration in a component or attachment on the tile.
- Provide a shared GUI widget and a Configurator tool that opens the side config screen directly.
- Support default configurations that can be overridden per face.
- Make the configuration serializable so it can be copied by config cards.
- Machines declare which transmission types they support; the side config UI only shows relevant options.

## 6. Chemical Tank and Network Design

### Goals
- Chemicals are first-class resources with clear storage and transfer rules.
- Consistent behavior across machines and tubes.

### Recommended Approach
- Chemical stacks carry type and amount (long-based).
- Tanks have capacity, stored amount, and optional filtering.
- Sided handlers expose insert/extract with face awareness.
- Pressurized tubes form networks with capacity and throughput limits.
- Support both push and pull modes.
- Chemical production machines (electrolytic separator, chemical infuser, etc.) feed into the same tank/network system.
- Keep chemical networks separate from fluid and item networks.

## 7. Factory Parallel Processing

### Goals
- Multiple process slots share recipe logic, energy, and chemical inputs efficiently.
- Auto-sort reduces manual management.

### Recommended Approach
- Base factory tile manages an array of process slots (input, output, progress).
- Slot count is determined by tier (3/5/7/9).
- Recipe lookup is shared; each slot tracks its own progress and input.
- Energy and chemical consumption are summed across active slots.
- Auto-sort moves matching items into empty slots when inventory changes.
- Factories reuse the same recipe handlers as their single-slot counterparts.

## 8. Energy Network Model

### Goals
- Predictable energy movement with clear capacity vs. throughput.
- Works for both simple machines and large storage.

### Recommended Approach
- Energy containers expose capacity and current amount.
- Transmitters have throughput limits and optional buffers.
- Networks track total capacity and current throughput.
- Machines can push energy or allow pull.
- Large storage (Induction Matrix style) separates storage cells (capacity) from providers (transfer rate).
- Document the unit (Joules, long-based) and any conversion rules early.

## 9. Debug / Network Reader Equivalent

### Goals
- Early visibility into structure status, network health, and formation failures.

### Recommended Approach
- Add a lightweight debug tool or command early in development.
- On use, display:
  - Whether a multiblock is formed and its size.
  - Network transmitter count, capacity, and throughput.
  - Last formation failure reason (if any).
  - Energy or chemical levels on the targeted block.
- Gate the tool behind a config flag or creative-only access during early development.

## 10. JEI/EMI Multiblock Preview Approach

### Goals
- Let players see structure requirements and costs before building.

### Recommended Approach
- Define structure requirements in data (min/max size, required blocks, internal components).
- Provide a JEI/EMI category or info page that reads this data.
- Show material counts and a simplified diagram or description.
- Avoid hard-coding layouts; derive counts from the validator rules where possible.

## 11. Config Card / Copy-Paste System

### Goals
- Reduce repetitive configuration of machines.

### Recommended Approach
- A config card item that can copy side configuration, upgrades, and security settings.
- On paste, apply settings to the target machine if it supports the same transmission types.
- Support batch operations on factories where practical.
- Store the copied data as a simple serializable object.

## 12. Phased Implementation Checklist

Recommended order for shared frameworks before individual machines:

1. PowerAPI energy containers and basic transfer.
2. Chemical stacks, tanks, and sided handlers.
3. Shared machine base with side configuration and upgrades.
4. Multiblock framework (formation, invalidation, data, cache).
5. Transmitter networks (energy, chemical, item).
6. Factory parallel slot system.
7. Debug tools.
8. Individual machines and structures.

## 13. Testing Strategy for Multiblocks

### Goals
- Verify formation, invalidation, and edge cases without full integration tests for every structure.

### Recommended Approach
- Unit-test validators with synthetic block maps.
- Test formation and invalidation with small structures in a controlled environment.
- Cover edge cases: rapid placement, max size, mixed-tier internals, shared faces, chunk boundaries.
- Use the debug tool during manual testing to confirm state.
- Add regression tests when formation bugs are fixed.

## 14. Data-Driven Balance Sheet

### Goals
- Allow balance changes without code edits.
- Let design notes feed directly into data.

### Recommended Approach
- Store tier multipliers, energy costs, processing speeds, and chemical ratios in config or data files.
- Machines and structures read these values at initialization.
- Provide a clear mapping from design documents to data keys.
- Use data generation where recipes are numerous.

---

These guidelines complement the existing clean-room analysis and formation protocol documents. They are intended to reduce rework and provide a clear path for implementation.
