# Client/Server Sync Strategy

## Goals
- Efficient syncing of tanks, energy, side config, and multiblock state.
- Rate-limiting to reduce packet load.

## Approach
- Reduced update tags for frequent changes.
- Sync on significant delta or timed intervals.
- Multiblock controllers sync aggregated state.
- Networks rebuild on load; no persistent network sync required.
- Debug tool can force full sync for inspection.
