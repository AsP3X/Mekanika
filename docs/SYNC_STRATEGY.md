# Client/Server Sync Strategy

## Goals
- Efficient syncing of tanks, energy, side config, multiblock state, and networks.
- Minimize packet load while keeping GUIs responsive.

## What to Sync
- Energy levels and rates (rate-limited).
- Chemical and fluid tank contents.
- Side configuration changes.
- Multiblock formation status and aggregated stats.
- Machine progress and status.
- Network visual activity (optional, client-side).

## Approach
- Use reduced update tags or delta sync where possible.
- Sync on significant change or timed intervals.
- Multiblock controllers sync aggregated state.
- Networks rebuild on load; no persistent network sync required.
- Debug tool can force a full sync for inspection.

## Rate Limiting
- Energy and tank levels: every few ticks or on large delta.
- Side config and formation: immediate on change.
- Progress bars: smooth client-side interpolation where practical.

## Edge Cases
- Rapid changes while GUI is open.
- Chunk unload/reload.
- Multiple players viewing the same machine.
- Large multiblocks with many stats.

## Implementation Notes
- Prefer block entity update packets with reduced tags.
- Cache last synced values to avoid redundant packets.
- Document sync frequency expectations.
