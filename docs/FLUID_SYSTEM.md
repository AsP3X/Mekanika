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

- Tiered storage: Basic → Advanced → Elite → Ultimate (capacity and max output roughly double each tier).
- Creative tier for infinite storage/output (optional).
- Auto-eject flag (pushes from bottom by default, configurable via side config).
- Bucket mode (optional): allows the tank item to place or scoop fluids when enabled.
- Input and output item slots for filling/draining tanks from buckets or portable tanks.
- Side configuration support (input / output / none / pull per face).
- Redstone control and security integration.
- Tanks retain contents when broken and replaced.

## Networks (Mechanical Pipes)

- Mechanical Pipes form fluid networks.
- Networks track:
  - Total capacity (buffer).
  - Throughput / pump rate.
  - Current fluid contents (prefer single type per network for simplicity).
- Connection modes (via Configurator):
  - Normal
  - Push
  - Pull
  - None (disconnected)
- Networks merge and split cleanly.
- Rate limiting and buffering prevent overflow and reduce tick cost.
- In-world upgrading of pipes is supported (alloy right-click upgrades connected pipes).

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
8. Support in-world pipe upgrading for quality-of-life.
9. Prefer single-fluid networks for simplicity and performance.

## Edge Cases

- Empty vs. partially filled tanks during network merges.
- Rapid fill/drain that exceeds throughput.
- Creative tanks interacting with finite networks.
- Bucket mode vs. placeable tank mode.
- Chunk unload while fluids are in pipes or tanks.
- Config card copying of fluid-related side configs.
- Multiple fluid types in the same network (if supported).

## Performance Considerations

- Rate-limit network updates and transfers.
- Cache acceptor lists.
- Prefer simulation before execution for large transfers.
- Avoid unnecessary fluid stack allocations.

This design allows machines, pipes, tanks, and multiblocks to share a consistent fluid system without tight coupling to the chemical or energy systems.
