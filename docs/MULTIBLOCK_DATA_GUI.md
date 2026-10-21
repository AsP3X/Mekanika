# Multiblock Data GUI

This document defines a general GUI pattern for displaying MultiblockData across all multiblock structures.

## Goals

- Provide a consistent way to view formation status, component counts, and aggregated statistics.
- Work for any multiblock (Dynamic Tank, Induction Matrix, Turbine, Reactor, etc.).
- Support debugging and player feedback.
- Integrate with structure-specific GUIs.

## Layout

### Overview Tab / Main View
- **Formation Status**: Formed / Unformed / Error, with last failure reason if applicable.
- **Size**: Dimensions (L × W × H) and total blocks.
- **Component Summary**: Counts of key internal components (cells, providers, blades, fuel assemblies, etc.).
- **Aggregated Stats**: Structure-specific values (capacity, rate, efficiency, temperature, etc.).
- **Controller Position**: Optional display of controller coordinates.

### Debug / Details Tab (optional)
- Full component list or breakdown by type.
- Last validation failure position and reason.
- Cache status or rebuild information.
- Raw data useful for developers or advanced players.

## Widgets

- Status indicator (colored light or text).
- Component counters with icons or labels.
- Key-value list for aggregated stats.
- Scrollable area for detailed component lists if needed.
- Link or button to structure-specific configuration (ports, valves).

## Interaction

- Opened from the controller or any structural block.
- Can be a dedicated tab in structure-specific GUIs or a shared screen.
- Debug tool can open or mirror this information.
- Read-only by default; configuration happens in dedicated sections.

## Integration

- Reads directly from MultiblockData and the spatial cache.
- Structure-specific GUIs can embed or extend this view.
- Syncs formation status and key stats efficiently.
- Works for both formed and unformed structures.

## Edge Cases

- Unformed structure (show error and failure reason).
- Very large structures with many component types.
- Rapid formation/invalidation while GUI is open.
- Security restrictions on viewing.

## Implementation Notes

- Create a reusable MultiblockData panel or widget.
- Structure-specific GUIs compose this panel with their own widgets.
- Rate-limit sync for dynamic values.
- Keep the debug view optional or gated.

## Player Feedback

- Clear formed/unformed status.
- Component counts so players can verify their build.
- Aggregated stats that match the structure’s purpose.
- Last failure reason when the structure is invalid.

This GUI provides a consistent, informative view of MultiblockData for all structures while allowing each structure to add its own specialized controls.
