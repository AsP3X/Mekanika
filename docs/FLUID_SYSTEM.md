# Fluid System API — Clean-Room Design

This document defines the conceptual design for Mekanika’s fluid system. All content is original and based on observed public patterns. No code or assets are reproduced.

## Overview

Fluids are first-class resources stored in tanks and transferred via Mechanical Pipes. The system is parallel to but separate from the chemical system.

## Core Concepts

- **FluidStack**: Type + amount (int or long, in mB). Returned stacks should not be mutated by callers.
- **IFluidTank** / **IExtendedFluidTank**:
  - Capacity, current amount, and type.
  - fill / drain (with Action: EXECUTE or SIMULATE).
  - setFluid / setEmpty.
  - onContentsChanged callback.
  - Optional filtering and rate limits.
- **IFluidHandler**: Multi-tank interface with sided support.
  - getTanks().
  - getFluidInTank(int).
  - fill / drain with side awareness.
  - Full simulation support.

## Tank Features

- Tiered storage: Basic, Advanced, Elite, Ultimate. Capacities are 32,000, 64,000, 128,000, and 256,000 mB. Per-tick rates are 1,000, 4,000, 16,000, and 64,000 mB. The block ids and the tick rule are in [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md).
- No creative fluid tank is registered.
- Default faces: input on the top and the four sides, output on the bottom.
- One fill slot and one drain slot. The tank item does not place or scoop a fluid on use.
- Tanks retain contents on the dropped item.
- Redstone control and security integration.

## Networks (Mechanical Pipes)

Mechanical pipes use the per-block buffer and the fair split in [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md). A pipe holds one fluid id. A different id does not connect, and neither buffer is deleted. Face modes are `normal`, `push`, `pull`, and `none`. A tier installer changes the pipe tier and keeps the buffer, clamped to the new capacity. Alloy right-click does not upgrade a pipe.

## Production and Consumption Patterns

- Machines declare input and output fluid tanks.
- Recipe system specifies fluid inputs/outputs with amounts.
- Common uses:
  - Water for electrolysis and cooling.
  - Brine for chemical production.
  - Coolants in reactors and turbines.
  - Outputs from pumps and separators.
- Fluid consumption is per-tick or per-operation, scaled by speed upgrades.
- Dynamic Tanks (multiblock) provide large-scale fluid storage using the same tank contracts.

## Integration Points

- **Machines**: Electrolytic Separator, pumps, reactors, boilers, etc., use the same tank and handler interfaces.
- **Transmitters**: Mechanical Pipes use the fluid network system.
- **Storage**: Fluid Tanks and Dynamic Tanks use the same contracts.
- **Side Configuration**: Fluid input/output is configured per face.
- **JEI/EMI**: Display fluid inputs/outputs and tank capacities.
- **Data generation**: Recipes and tags for fluids are data-driven.

## Implementation Recommendations

1. Define fluid tank and handler interfaces early (or reuse standard fluid APIs with extensions).
2. Implement a basic tank class with capacity, fill/drain, and change notifications.
3. Support sided handlers from the start so side config works immediately.
4. Keep fluid inventories completely separate from chemical and item systems.
5. Document the unit (mB) and any conversion rules clearly.
6. Add auto-eject and connection modes early.
7. Provide a debug tool that shows fluid network contents and tank levels.
8. Use the tier installer for pipe upgrades. Keep the buffer and delete only the amount above the new capacity.
9. One fluid id per pipe. A refused connection deletes nothing.

## Edge Cases

- An empty pipe connects to a pipe that already holds a fluid. The link step can move fluid into the empty pipe.
- Rapid fill/drain that exceeds throughput. The pipe accepts at most its throughput, and at most its free buffer.
- No creative tank is registered. A finite tank stops at its capacity.
- The fill and drain slots move buckets. Use on a block does not place or scoop a fluid.
- Chunk unload while fluids are in pipes or tanks. The buffer stays on the block.
- Config card copying of fluid-related side configs.
- Two pipes holding different fluids do not connect. Both amounts stay.

## Performance Considerations

- Rate-limit network updates and transfers.
- Cache acceptor lists.
- Prefer simulation before execution for large transfers.
- Avoid unnecessary fluid stack allocations.

This design allows machines, pipes, tanks, and multiblocks to share a consistent fluid system without tight coupling to the chemical or energy systems.
