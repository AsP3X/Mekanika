# Mekanika Documentation Index

**Start here**: [PLAN.md](PLAN.md) for the overall project plan, then [FRAMEWORK_IMPLEMENTATION_ORDER.md](FRAMEWORK_IMPLEMENTATION_ORDER.md) for the recommended coding sequence.

Recommended reading order for implementers:

## Core Systems
- [ENERGY_POWERAPI.md](ENERGY_POWERAPI.md) — Energy unit, containers, networks, Fabric notes (canonical)
- [POWERAPI_DESIGN.md](POWERAPI_DESIGN.md) — Earlier energy design notes (see ENERGY_POWERAPI.md for the latest)
- [CHEMICAL_SYSTEM.md](CHEMICAL_SYSTEM.md) — Chemical stacks, tanks, networks, production loops
- [FLUID_SYSTEM.md](FLUID_SYSTEM.md) — Fluid tanks, mechanical pipes, Dynamic Tanks
- [NETWORKING_API.md](NETWORKING_API.md) — Shared transmitter network design
- [ITEM_TRANSPORT.md](ITEM_TRANSPORT.md) — Logistical transporters, sorting, filtering
- [HEAT_SYSTEM.md](HEAT_SYSTEM.md) — Heat transfer and thermodynamic conductors

## Machine Frameworks
- [SIDE_CONFIGURATION.md](SIDE_CONFIGURATION.md) — Per-face modes and Configurator
- [CONTROL_SYSTEM.md](CONTROL_SYSTEM.md) — Redstone, security, config cards
- [CONFIG_CARD_EDGE_CASES.md](CONFIG_CARD_EDGE_CASES.md) — Config card copy/paste edge cases
- [UPGRADE_SYSTEM.md](UPGRADE_SYSTEM.md) — Speed/energy/gas upgrades
- [MULTIBLOCK_FORMATION.md](MULTIBLOCK_FORMATION.md) — Formation protocol
- [MULTIBLOCK_INVALIDATION.md](MULTIBLOCK_INVALIDATION.md) — Invalidation and reformation

## Advanced Systems
- [QIO_SYSTEM.md](QIO_SYSTEM.md) — Frequency-based storage
- [GEAR_MODULE_SYSTEM.md](GEAR_MODULE_SYSTEM.md) — MekaSuit modules
- [RADIATION_SYSTEM.md](RADIATION_SYSTEM.md) — Exposure and protection

## Process and Support
- [FRAMEWORK_IMPLEMENTATION_ORDER.md](FRAMEWORK_IMPLEMENTATION_ORDER.md) — Coding sequence (canonical)
- [FRAMEWORK_ORDER.md](FRAMEWORK_ORDER.md) — Earlier framework notes (see FRAMEWORK_IMPLEMENTATION_ORDER.md)
- [IMPLEMENTATION_GUIDANCE.md](IMPLEMENTATION_GUIDANCE.md) — Practical design notes
- [TESTING_STRATEGY.md](TESTING_STRATEGY.md) — Unit and manual testing
- [DEBUG_TOOL_SPEC.md](DEBUG_TOOL_SPEC.md) — Network reader outputs
- [FAILURE_MODES.md](FAILURE_MODES.md) — Player-facing failure states
- [POWER_BUDGET.md](POWER_BUDGET.md) — Progression energy targets
- [PUBLIC_API_SUMMARY.md](PUBLIC_API_SUMMARY.md) — Stable interfaces
- [SYNC_STRATEGY.md](SYNC_STRATEGY.md) — Client/server sync rules
- [GUI_PATTERNS.md](GUI_PATTERNS.md) — Shared GUI conventions
- [PERFORMANCE_GUIDELINES.md](PERFORMANCE_GUIDELINES.md) — Tick and network performance rules
- [NBT_CONVENTIONS.md](NBT_CONVENTIONS.md) — Data component and NBT standards
- [RECIPE_DATA_GENERATION.md](RECIPE_DATA_GENERATION.md) — Recipes and data generation (canonical)
- [RECIPE_DATA_GEN.md](RECIPE_DATA_GEN.md) — Earlier recipe notes (see RECIPE_DATA_GENERATION.md)
- [TEXTURE_HANDOFF.md](TEXTURE_HANDOFF.md) / [TEXTURE_HANDOFF_EXPANDED.md](TEXTURE_HANDOFF_EXPANDED.md) — Texture requirements
- [CLEAN_ROOM_ANALYSIS.md](CLEAN_ROOM_ANALYSIS.md) — Architectural learnings
- [ADDITIONAL_SYSTEMS.md](ADDITIONAL_SYSTEMS.md) — Miscellaneous notes
- [PLAN.md](PLAN.md) — Overall project plan

All documents are clean-room and intended for implementation by Grok with design support from Claude.
