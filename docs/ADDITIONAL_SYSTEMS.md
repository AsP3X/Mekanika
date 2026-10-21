# Additional High-Value Systems — Clean-Room Notes

## Internal Component Aggregation
- On successful formation, collect internal components (cells, providers, coils, fuel assemblies).
- Aggregate stats (capacity, rate, efficiency) into MultiblockData.
- Support mixed tiers.

## Controller and Bounding Blocks
- Controller owns MultiblockData and opens the GUI.
- Bounding/proxy blocks forward interactions to the controller.
- Only one controller allowed per structure.

## Persistence
- Cache formed structure positions and key data.
- Recalculate derived stats on load where cheap.
- Sync reduced data to clients.

## Factory Parallel Processing
- Shared recipe logic across slots.
- Per-slot progress or shared progress depending on design.
- Energy and chemical inputs distributed or shared.

## Energy Network Model
- Capacity (buffer) vs transfer rate.
- Push preferred; pull modes supported.
- Networks merge and split cleanly.

## Debug Tools
- Early command or item that shows structure status, network stats, and last formation failure.

## JEI/EMI Previews
- Register multiblock recipes or uses with structure requirements and costs.
- Avoid hard-coding layouts in code.

## Config Cards
- Copy side config, upgrades, and security between machines.

## Testing Strategy
- Unit tests for validators and formation results.
- Game tests for rapid placement, mixed tiers, max size, and invalidation.
- Focus on edge cases early.

## Data-Driven Balance
- Store tier multipliers, chemical ratios, energy costs, and processing times in configs or data files.
- Claude’s balance notes feed directly into these.

All of the above should be implemented originally and documented as Mekanika-specific.
