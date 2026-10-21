# Energy System / PowerAPI Design for Mekanika

This document provides detailed, clean-room design guidance for the energy system. All content is original and conceptual.

## Goals

- Provide a consistent energy model across Fabric, Forge, and NeoForge.
- Support simple machines, large storage structures, and gear.
- Clearly separate capacity from transfer rate.
- Enable predictable push and pull behavior.
- Remain data-driven for balance values.
- Expose a clean public API for addons.

## Core Concepts

### Energy Unit
- Primary unit is the Joule (J), stored as a long.
- Document conversion rules early if compatibility with other systems (e.g., Forge Energy) is required.
- All internal calculations use the same unit to avoid repeated conversions.

### IEnergyContainer
- Capacity (long).
- Current stored amount.
- Optional maximum insert and extract rates.
- insert and extract methods with simulation support.
- onContentsChanged callback for syncing and network updates.
- getNeeded and getEnergy for querying.

### Handlers
- IStrictEnergyHandler for general energy access.
- Sided variants that respect face-based configuration.
- Support for both containers (storage) and transmitters (transfer).

### Conversion Helpers
- If cross-mod compatibility is needed, provide conversion utilities between Joules and external energy types.
- Keep conversion at the boundary; internal systems stay in Joules.

## Storage

### Basic Containers
- Machines and items (energy tablets, gear) use energy containers.
- Capacity and rates can be tiered or affected by upgrades.
- Containers support partial fills and report remaining capacity.

### Large Storage (Induction Matrix Style)
- Separate storage cells (contribute capacity) from providers (contribute transfer rate).
- Ports act as the network connection points.
- Aggregated capacity and rate are calculated at formation time.
- Mixed-tier internals are supported; contributions are summed.

## Networks and Transfer

### Transmitters
- Universal cables form energy networks.
- Each transmitter has a throughput limit and optional internal buffer.
- Networks track:
  - Total connected storage capacity.
  - Current buffer level.
  - Throughput supply and demand.
  - Connected acceptors and emitters.

### Transfer Modes
- Push: machine or storage ejects energy on configured output faces.
- Pull: network or machine requests energy on input faces.
- Controlled by side configuration.
- Networks distribute available energy fairly or by priority rules (document the chosen rule).

### Buffering
- Transmitters or networks may hold a small buffer to smooth transfer.
- Buffer size should be configurable or derived from transmitter tier.
- Avoid large buffers that hide capacity problems.

## Integration Points

- Machines consume energy per operation or per tick based on recipes and upgrades.
- Gear and modules draw energy for active abilities.
- Multiblock generators and reactors produce energy into the network via ports.
- Side configuration determines which faces accept or emit energy.
- Debug tool reports container levels, network throughput, and buffer status.

## Fabric-Specific Notes

- Fabric lacks a built-in energy API equivalent to Forge Energy.
- Implement a full custom energy system on Fabric.
- On Forge/NeoForge, either share the same implementation or provide thin bridges to existing energy systems.
- Keep the public API identical across loaders so common code does not branch on energy logic.

## Syncing and Persistence

- Energy levels in machines and storage are saved with the tile or item.
- Client sync is needed for GUI display (rate-limit frequent changes).
- Networks rebuild on load; persistent network state is not required.
- Large structures save aggregated energy on the controller.

## Performance Considerations

- Maintain acceptor and emitter lists to avoid full network scans every tick.
- Use simulation mode for checks.
- Batch transfers where multiple machines interact with the same network.
- Rate-limit network rebuilds during rapid cable placement.

## Player Feedback

- GUIs show current energy, capacity, and input/output rates.
- Connected cables visually indicate activity where practical.
- Debug tool provides network-wide statistics.
- Clear feedback when a machine cannot run due to insufficient energy.

## Implementation Recommendations

1. Define the energy unit and container interfaces first.
2. Implement basic storage and transfer before complex networks.
3. Add side configuration support early so machines can control energy faces.
4. Document conversion rules and Fabric implementation details in this file.
5. Expose a minimal public API for addons.
6. Keep capacity and transfer rate as separate concepts from the start.
7. Add the debug tool once basic networks function.

## Edge Cases

- Creative energy sources interacting with finite networks.
- Rapid connection and disconnection of cables.
- Machines with both input and output faces on the same network.
- Chunk unload while energy is in transit.
- Very large Induction Matrix style structures and their aggregation cost.

This design provides a solid foundation for all energy-using systems in Mekanika.
