# Energy System / PowerAPI Design for Mekanika

This document provides detailed, clean-room design guidance for the energy system. All content is original and conceptual. No external code or assets are referenced.

## Goals

- Provide a consistent energy model across Fabric, Forge, and NeoForge.
- Support simple machines, large storage structures, transmitters, and gear.
- Clearly separate capacity from transfer rate.
- Enable predictable push and pull behavior controlled by side configuration.
- Remain data-driven for balance values.
- Expose a clean public API for addons.
- Minimize tick cost for large networks.

## Core Concepts

### Energy Unit
- Primary unit is the Joule (J), stored as a long.
- Display formatting uses SI prefixes (kJ, MJ, GJ, TJ).
- Document conversion rules early if compatibility with other systems (e.g., Forge Energy) is required. Prefer simple ratios (1:1 or 1:2.5) and perform conversion only at the boundary.
- All internal calculations stay in Joules to avoid repeated conversions.

### IEnergyContainer
- Capacity (long).
- Current stored amount.
- Optional maximum insert and extract rates (per tick).
- insert and extract methods with Action support (EXECUTE vs SIMULATE).
- onContentsChanged callback for syncing, network updates, and redstone.
- getNeeded, getEnergy, getMaxEnergy for querying.
- isEmpty / isFull helpers.

### Handlers
- IEnergyHandler for general multi-container access.
- Sided variants that respect face-based configuration.
- Support for both storage containers and transmitters.
- Simulation mode must be accurate so networks can plan transfers without side effects.

### Conversion Helpers
- If cross-mod compatibility is needed, provide conversion utilities between Joules and external energy types.
- Keep conversion at the mod boundary; internal systems never mix units.

## Storage

### Basic Containers
- Machines, energy cubes, and items (energy tablets, gear) use energy containers.
- Capacity and rates can be tiered or affected by upgrades (data-driven multipliers).
- Containers support partial fills and report remaining capacity.
- Portable batteries (Energy Cubes) have both input and output rates.

### Large Storage (Induction Matrix Style)
- Separate storage cells (contribute capacity) from providers (contribute transfer rate).
- Ports act as the network connection points and respect side configuration.
- Aggregated capacity and rate are calculated at formation time and cached.
- Mixed-tier internals are supported; contributions are summed.
- Controller owns the aggregated energy value and handles tick logic.

## Networks and Transfer

### Transmitters
- Universal cables form energy networks.
- Each transmitter has a throughput limit (tiered) and optional small internal buffer.
- Networks track:
  - Total connected storage capacity.
  - Current buffer level.
  - Throughput supply and demand.
  - Connected acceptors and emitters.
  - Network ID for debugging.

### Transfer Modes
- Push: machine or storage ejects energy on configured output faces.
- Pull: network or machine requests energy on input faces.
- Controlled entirely by side configuration.
- Networks distribute available energy fairly or by a documented priority rule (e.g., proportional to demand).
- Rate limiting prevents single-tick spikes that could overflow or desync.

### Buffering
- Transmitters or networks may hold a small buffer to smooth transfer and reduce tick cost on long cables.
- Buffer size should be configurable or derived from transmitter tier.
- Avoid large buffers that hide underlying capacity problems.

### Network Lifecycle
- Networks form dynamically on cable placement and dissolve on break.
- Request-driven rebuilds (not every tick) with rate limiting.
- On chunk unload, networks cleanly dissolve and reform on load.
- Support for creative/infinite sources that do not drain.

## Integration Points

- Machines consume energy per operation or per tick based on recipes, upgrades, and active slots.
- Gear and modules draw energy for active abilities (flight, mining, shielding).
- Multiblock generators and reactors produce energy into the network via ports.
- Side configuration determines which faces accept or emit energy.
- Debug tool reports container levels, network throughput, buffer status, and connected node count.
- Redstone comparators can output based on energy fill level.

## Fabric-Specific Notes

- Fabric lacks a built-in energy API equivalent to Forge Energy.
- Implement a full custom energy system on Fabric (lookup or attachment-based).
- Provide a public API that other mods can depend on.
- On Forge/NeoForge, either share the same internal implementation or provide thin bridges to native energy systems.
- Keep the public API identical across loaders so common code does not branch on energy logic.
- Document the Fabric API surface clearly for third-party integration.

## Syncing and Persistence

- Energy levels in machines and storage are saved with the tile or item NBT.
- Client sync is needed for GUI display; rate-limit frequent changes (e.g., only on significant delta or every few ticks).
- Networks rebuild on load; persistent network state is not required beyond cable positions.
- Large structures save aggregated energy on the controller.
- Use reduced update tags where possible to minimize packet size.

## Performance Considerations

- Maintain acceptor and emitter lists to avoid full network scans every tick.
- Use simulation mode for pre-checks.
- Batch transfers where multiple machines interact with the same network.
- Rate-limit network rebuilds during rapid cable placement or destruction.
- Cache tier multipliers and rates so they are not recalculated every tick.
- Profile large networks early (hundreds of cables + many machines).

## Player Feedback

- GUIs show current energy, capacity, and input/output rates.
- Connected cables can visually indicate activity (particles or texture animation) where practical.
- Debug tool provides network-wide statistics and last failure reasons.
- Clear feedback when a machine cannot run due to insufficient energy (GUI message or status light).
- Energy Cubes and tablets show fill level in tooltips and GUIs.

## Implementation Recommendations

1. Define the energy unit and container interfaces first.
2. Implement basic storage and transfer before complex networks.
3. Add side configuration support early so machines can control energy faces.
4. Document conversion rules and Fabric implementation details in this file.
5. Expose a minimal public API for addons.
6. Keep capacity and transfer rate as separate concepts from the start.
7. Add the debug tool once basic networks function.
8. Implement large storage aggregation after the multiblock framework is stable.
9. Make all rates and capacities data-driven.

## Edge Cases

- Creative energy sources interacting with finite networks.
- Rapid connection and disconnection of cables.
- Machines with both input and output faces on the same network (loop prevention).
- Chunk unload while energy is in transit or buffered.
- Very large Induction Matrix style structures and their aggregation cost.
- Mixed-tier cables in the same network (throughput limited by weakest link or averaged).
- Overflow handling (prefer clamp or reject rather than silent loss unless designed).
- Energy tablets and gear charging from any configured face.

## Balance Considerations

- Tier multipliers for capacity and rate should live in config or data files.
- Early generators should be sufficient for basic machines but not late-game factories.
- Large storage should feel like a meaningful upgrade over stacks of Energy Cubes.
- Document expected power budgets for each progression stage.

This design provides a solid, scalable foundation for all energy-using systems in Mekanika.
