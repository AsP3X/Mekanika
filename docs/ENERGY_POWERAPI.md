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
- Forge and NeoForge convert at the capability boundary. The ratio is 5 Forge Energy for every 2 Joules, the public 2.5 FE per Joule, done with integers. Fabric does not expose Forge Energy.
- All internal calculations stay in Joules.
- Prefer integer arithmetic throughout.

### IEnergyContainer
- Capacity (long).
- Current stored amount.
- Optional maximum insert and extract rates (per tick).
- insert and extract methods with Action support (EXECUTE vs SIMULATE).
- onContentsChanged callback for syncing, network updates, and redstone.
- getNeeded, getEnergy, getMaxEnergy for querying.
- isEmpty / isFull / canReceive / canExtract helpers.
- Optional: getStoredPercentage for GUI and comparator.

### Handlers
- IEnergyHandler for general multi-container access.
- Sided variants that respect face-based configuration.
- Support for both storage containers and transmitters.
- Simulation mode must be accurate so networks can plan transfers without side effects.
- Optional: getContainerForSide(Direction).

### Conversion Helpers
- If cross-mod compatibility is needed, provide conversion utilities between Joules and external energy types.
- Keep conversion at the mod boundary; internal systems never mix units.
- Make conversion configurable.

## Storage

### Basic Containers
- Machines, energy cubes, and items (energy tablets, gear) use energy containers.
- Capacity and rates can be tiered or affected by upgrades (data-driven multipliers).
- Containers support partial fills and report remaining capacity.
- Portable batteries (Energy Cubes) have both input and output rates.
- Energy tablets are stackable items with their own energy storage.

### Large Storage (Induction Matrix Style)
- Separate storage cells (contribute capacity) from providers (contribute transfer rate).
- Ports act as the network connection points and respect side configuration.
- Aggregated capacity and rate are calculated at formation time and cached in MultiblockData.
- Mixed-tier internals are supported; contributions are summed.
- Controller owns the aggregated energy value and handles tick logic.
- Bounding blocks proxy interactions to the controller.

## Networks and Transfer

### Transmitters
- Universal cables form energy networks.
- Each cable's buffer equals its throughput. The tick rule is in [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md).
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
- A cable stores its own buffer, equal to its throughput. The fair split and the link step are in [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md).
- Rate limiting prevents single-tick spikes that could overflow or desync.

### Buffering
- A universal cable's buffer is its throughput key: 1,000, 8,000, 32,000, or 128,000 J. The network does not keep a second copy.

### Network Lifecycle
- Networks form dynamically on cable placement and dissolve on break.
- Request-driven rebuilds (not every tick) with rate limiting.
- On chunk unload, networks cleanly dissolve and reform on load.
- Support for creative/infinite sources that do not drain.
- Orphan transmitters are assigned on the next tick.

## Integration Points

- Machines consume energy per operation or per tick based on recipes, upgrades, and active slots.
- Gear and modules draw energy for active abilities (flight, mining, shielding).
- Multiblock generators and reactors produce energy into the network via ports.
- Side configuration determines which faces accept or emit energy.
- Debug tool reports container levels, network throughput, buffer status, and connected node count.
- Redstone comparators can output based on energy fill level.
- Factories sum energy consumption across active parallel slots.

## Fabric-Specific Notes

- Fabric lacks a built-in energy API equivalent to Forge Energy.
- Implement a full custom energy system on Fabric (lookup or attachment-based).
- Provide a public API that other mods can depend on.
- Forge and NeoForge keep the same Joule containers and add a Forge Energy bridge at the ratio below.
- Keep the public API identical across loaders so common code does not branch on energy logic.
- Document the Fabric API surface clearly for third-party integration.
- Consider an optional Fabric energy API dependency for broader compatibility.

## Syncing and Persistence

- Energy levels in machines and storage are saved with the tile or item NBT.
- Client sync is needed for GUI display; rate-limit frequent changes (e.g., only on significant delta or every few ticks).
- Networks rebuild on load; persistent network state is not required beyond cable positions.
- Large structures save aggregated energy on the controller.
- Use reduced update tags where possible to minimize packet size.
- Energy tablets and gear save their energy in item NBT.

## Forge Energy bridge

Forge and NeoForge only. The bridge sits on the block or item that already stores Joules. Callers see Forge Energy. The container still stores Joules.

Defaults: `energy.forge.fe_numerator = 5` and `energy.forge.joule_denominator = 2`. Both are integers, at least 1. The keys live in `mekanika-energy.toml`.

Joules inserted toward Forge Energy:

`units = joules * fe_numerator + j_to_fe_carry`

`fe = units / joule_denominator`

`j_to_fe_carry = units % joule_denominator`

Forge Energy inserted toward Joules:

`units = fe * joule_denominator + fe_to_j_carry`

`joules = units / fe_numerator`

`fe_to_j_carry = units % fe_numerator`

The carries are saved on that block or item: `mekanika:j_to_fe_carry` and `mekanika:fe_to_j_carry`. A zero carry is omitted. A simulate call does not change the carry.

Two Joules become 5 FE with both carries left at 0. One Joule becomes 2 FE and leaves `j_to_fe_carry` at 1. The next Joule then becomes 3 FE and clears that carry. Five separate 1 FE inserts become 2 Joules: the third insert yields 1 Joule, the fifth yields the other, and `fe_to_j_carry` ends at 0. One thousand Joules, a basic enrichment tick, become 2,500 FE.

## Normative constants

```normative
energy.forge.fe_numerator=5
energy.forge.joule_denominator=2
energy.forge.fabric_bridge=0
```

## Worked-example index

```normative-examples
example.forge.two_j=5
example.forge.one_j_fe=2
example.forge.one_j_carry=1
example.forge.next_j_fe=3
example.forge.five_fe=2
example.forge.thousand_j=2500
example.forge.fabric_bridge=0
```

## Performance Considerations

- Maintain acceptor and emitter lists to avoid full network scans every tick.
- Use simulation mode for pre-checks.
- Batch transfers where multiple machines interact with the same network.
- Rate-limit network rebuilds during rapid cable placement or destruction.
- Cache tier multipliers and rates so they are not recalculated every tick.
- Profile large networks early (hundreds of cables + many machines).
- Prefer long-packed positions and open hash sets for network data.

## Player Feedback

- GUIs show current energy, capacity, and input/output rates.
- Connected cables can visually indicate activity (particles or texture animation) where practical.
- Debug tool provides network-wide statistics and last failure reasons.
- Clear feedback when a machine cannot run due to insufficient energy (GUI message or status light).
- Energy Cubes and tablets show fill level in tooltips and GUIs.
- Comparator output for energy fill percentage.

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
10. Test with long cable runs and mixed-tier networks early.

## Edge Cases

- Creative energy sources interacting with finite networks.
- Rapid connection and disconnection of cables.
- Machines with both input and output faces on the same network (loop prevention).
- Chunk unload while energy is in transit or buffered.
- Very large Induction Matrix style structures and their aggregation cost.
- Mixed-tier cables use the link rate `min` of the two throughputs. A full basic cable beside an empty ultimate cable moves 992 J.
- Overflow handling (prefer clamp or reject rather than silent loss unless designed).
- Energy tablets and gear charging from any configured face.
- Zero-capacity or zero-rate containers.
- Simultaneous push and pull on the same network.
- Desync between client display and server energy levels.

## Balance Considerations

- Tier multipliers for capacity and rate should live in config or data files.
- Early generators should be sufficient for basic machines but not late-game factories.
- Large storage should feel like a meaningful upgrade over stacks of Energy Cubes.
- Document expected power budgets for each progression stage.
- Energy costs for machines should scale with speed upgrades.

This design provides a solid, scalable foundation for all energy-using systems in Mekanika.
