# Networking / Transmitter API — Clean-Room Design

This document defines the conceptual design for Mekanika’s transmitter and network systems. All content is original. No code or assets are reproduced.

## Overview

Transmitters form dynamic networks that move resources between machines. Supported resource types:
- Energy (Universal Cables)
- Chemicals (Pressurized Tubes)
- Fluids (Mechanical Pipes)
- Items (Logistical Transporters)
- Heat (Thermodynamic Conductors)

Each resource type has its own blocks. Energy, chemical, and fluid amounts, block ids, and the per-tick split are in [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md). Items and heat stay in their own documents.

## Core Concepts

- **Transmitter**: A block that stores its own buffer and six face modes. It does not save a network id.
- **Network**: The transmitters that can reach each other under the connection rule. Debug reports the count. The network does not store a second buffer.
- **DynamicNetwork**: Base network with transmitters, acceptors, and merge/split logic.
- **DynamicBufferedNetwork**: Extends DynamicNetwork for resources that buffer (energy, chemicals, fluids, heat).
- **Orphan handling**: A new transmitter transfers by reading its six neighbors on the same tick. The debug cache assigns it on a later tick, at most 8 network rebuilds a tick.

## Network Lifecycle

1. **Placement**: Transmitter is placed and marked as orphan.
2. **Assignment**: On tick, orphan transmitters scan neighbors and either join an existing network or create a new one.
3. **Merge**: If two networks become connected, the smaller merges into the larger.
4. **Split**: If a transmitter is broken, the network is split and remaining transmitters re-form as needed.
5. **Unload**: Networks are cleaned up when chunks unload. Orphans and invalid transmitters are removed.

## Shared Network Features

- Each transmitter's own buffer and throughput, from [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md).
- Current contents on that block.
- Acceptor and emitter lists (machines connected to the network).
- Rate limiting to prevent lag.
- Events for transfer (for debugging and visual feedback).
- Side configuration support for pull/push and filtering.

## Resource-Specific Behavior

- **Energy**: Long joules. One buffer per cable. The split is in [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md).
- **Chemicals**: One id per tube. A different id does not connect, and neither buffer is deleted.
- **Fluids**: One id per pipe. A different id does not connect, and neither buffer is deleted.
- **Items**: Pathfinding with color channels, filters, and sorting. Supports pull mode and diversion.
- **Heat**: Transfers heat between blocks. Used by boilers, turbines, and reactors.

## Implementation Recommendations

1. Implement a shared DynamicNetwork base class early.
2. Add DynamicBufferedNetwork for energy, chemicals, fluids, and heat.
3. Handle orphans and network assignment on a dedicated tick or rate-limited pass.
4. Prefer merging into the larger network for performance.
5. Cache acceptor lists and only rebuild on network changes.
6. Provide a Network Reader debug tool that shows network size, capacity, throughput, and contents.
7. Support side configuration on transmitters for directionality and filtering.
8. Rate-limit network updates and transfers.
9. Clean up networks properly on chunk unload to avoid memory leaks.
10. Keep resource networks completely separate from each other.

## Performance Considerations

- Rate-limit formation and merge checks.
- Use compact data structures (long-packed positions, open hash sets).
- Cache chunk references during network scans.
- Avoid tile entity lookups when block state checks suffice.
- Batch transfers where possible.
- Limit the number of network updates processed per tick.

## Edge Cases

- Rapid placement of many transmitters.
- Breaking a cable in the middle of a long network.
- Mixed-tier transmitters in the same network.
- Chunk boundaries and unload/reload.
- Creative sources interacting with finite networks.
- Circular networks and pathfinding for items.
- Filtering and color channels on logistical transporters.

## Debug and Player Feedback

- Network Reader item or command shows:
  - Network type and size.
  - Capacity, buffer, and throughput.
  - Connected machines.
  - Last transfer amount.
- Visual feedback (particles or texture animation) for active networks.
- Clear error messages when networks fail to form or transfer.

This design provides a consistent, performant foundation for all transmitter types while allowing resource-specific behavior where needed.
