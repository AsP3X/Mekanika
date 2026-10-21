# PowerAPI Design Notes — Expanded Clean-Room

This document defines the conceptual design for Mekanika’s energy system. All content is original.

## Energy Unit

- Name: Joules (J) or project-chosen equivalent.
- Storage type: long (to support large values without overflow).
- Display: Formatted with SI prefixes (kJ, MJ, GJ, etc.).
- Conversion: Document clear conversion rates to other energy systems (e.g., Forge Energy) if bridges are provided. Prefer 1:1 or simple ratios.

## Core Interfaces

- **IEnergyContainer**:
  - getEnergy() / getMaxEnergy() (long).
  - insert / extract (with Action: EXECUTE or SIMULATE).
  - setEnergy / setEmpty.
  - onContentsChanged callback.
  - Optional: getMaxInsert / getMaxExtract for rate limiting.
- **IEnergyHandler** (multi-container, sided):
  - getEnergyContainers().
  - insertEnergy / extractEnergy with side awareness.
  - Support for simulation.

## Containers and Storage

- Machines and batteries hold one or more energy containers.
- Capacity and transfer rates are tiered and data-driven (config or data files).
- Energy Cubes act as portable batteries with input/output rates.
- Large storage (Induction Matrix style) separates capacity (cells) from transfer rate (providers).

## Transfer Model

- Preferred: Push-based (machines and cables eject energy).
- Supported: Pull mode via side configuration.
- Networks track:
  - Total buffer capacity.
  - Current energy level.
  - Throughput limit.
- Cables have per-cable throughput and optional buffer.
- Rate limiting prevents single-tick spikes.
- Events for energy transfer enable debugging and integration.

## Loader Strategy

- **Fabric**: Full custom implementation using a lookup or attachment system equivalent to capabilities. Provide an energy API that other mods can depend on.
- **Forge / NeoForge**: Bridge to native energy systems (Forge Energy) or share the same internal implementation for consistency across loaders.
- Architectury or similar for multi-loader support where practical.
- Document the public API surface early so addons can integrate cleanly.

## Network Behavior

- Energy cables form dynamic networks that merge and split on placement/break.
- Networks distribute energy fairly or by priority (configurable).
- Support for creative/infinite energy sources that do not drain.
- Buffering in cables reduces tick cost for long networks.

## Implementation Recommendations

1. Define IEnergyContainer and IEnergyHandler in the PowerAPI early.
2. Implement a basic container class with capacity, insert/extract, and change notifications.
3. Support sided handlers from the start for side config compatibility.
4. Keep energy separate from chemical and fluid systems.
5. Document the unit, display formatting, and any conversion rules clearly.
6. Add a debug tool that shows energy levels, network capacity, and throughput.
7. Make rates and capacities data-driven for easy balancing.

## Edge Cases

- Creative energy sources interacting with finite networks.
- Rapid insertion exceeding throughput.
- Chunk unload with energy in cables.
- Mixed-tier cables in the same network.
- Energy loss or overflow handling (prefer no loss unless designed).

This design should allow machines, cables, storage, and generators to share a consistent energy system across all loaders.
