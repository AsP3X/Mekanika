# Config Structure for Mekanika

This document defines how configuration is organized and which values are exposed. All content is original and conceptual.

## Goals

- Keep balance and gameplay values data-driven.
- Organize configs clearly for server operators.
- Support reloading where safe.
- Map design notes directly to keys.

## Organization

- **General**: debug mode, creative overrides, default security mode.
- **Energy**: cube capacities, cable rates, tier multipliers.
- **Machines**: processing times, energy costs per machine type.
- **Chemicals / Fluids**: tank capacities, pipe/tube rates.
- **Generators / Reactors**: outputs, fuel consumption, temperature thresholds.
- **World**: ore generation settings.
- **Client**: particle density, sound volume, HUD options.
- **Networks**: throughput limits, rebuild rate limits.

## Example Keys

- `general.debugMode`
- `energy.cube.basic.capacity`
- `energy.cable.advanced.throughput`
- `machine.enrichment.energyPerTick`
- `machine.enrichment.ticksPerOperation`
- `chemical.tank.basic.capacity`
- `generator.heat.output`
- `reactor.fission.maxTemperature`
- `worldgen.ore.veinSize`
- `client.particles.enabled`

## Format and Behavior

- Prefer standard config format (TOML or JSON) with comments.
- Safe defaults for all keys.
- Version or migration notes if the format changes.
- Reload support for non-structural values where practical.
- Data packs can override recipes independently of config.

## Implementation Notes

- Define the config class early.
- Group related values logically.
- Test extreme values (zero, very large).
- Provide a commented sample config.
- Document keys in POWER_BUDGET.md or inline comments.

## Edge Cases

- Missing keys (use defaults).
- Invalid values (clamp or reject with log).
- Conflicting data pack and config values (document precedence).

Clear config structure supports balance iteration and server customization.
