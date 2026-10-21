# Mekanika Documentation Index

**Start here**: [PLAN.md](PLAN.md) for the overall project plan, then [FRAMEWORK_IMPLEMENTATION_ORDER.md](FRAMEWORK_IMPLEMENTATION_ORDER.md) for the recommended coding sequence.

Recommended reading order for implementers:

## Core Systems
- [ENERGY_POWERAPI.md](ENERGY_POWERAPI.md) — Energy unit, containers, networks, Fabric notes (canonical)
- [POWERAPI_DESIGN.md](POWERAPI_DESIGN.md) — Earlier energy design notes (see ENERGY_POWERAPI.md)
- [CHEMICAL_SYSTEM.md](CHEMICAL_SYSTEM.md) — Chemical stacks, tanks, networks, production loops
- [FLUID_SYSTEM.md](FLUID_SYSTEM.md) — Fluid tanks, mechanical pipes, Dynamic Tanks
- [NETWORKING_API.md](NETWORKING_API.md) — Shared transmitter network design
- [ITEM_TRANSPORT.md](ITEM_TRANSPORT.md) — Logistical transporters, sorting, filtering
- [HEAT_SYSTEM.md](HEAT_SYSTEM.md) — Heat transfer and thermodynamic conductors
- [TRANSMITTER_CONFIG.md](TRANSMITTER_CONFIG.md) — Transmitter color, filter, and connection modes
- [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md) — Cable, tube, and pipe buffers, fluid tanks, and the per-tick split

## Machine Frameworks
- [SIDE_CONFIGURATION.md](SIDE_CONFIGURATION.md) — Per-face modes and Configurator
- [SIDE_CONFIG_SCREEN.md](SIDE_CONFIG_SCREEN.md) — Dedicated side configuration screen
- [CONTROL_SYSTEM.md](CONTROL_SYSTEM.md) — Redstone, security, config cards
- [SECURITY_DESK_GUI.md](SECURITY_DESK_GUI.md) — Security Desk and trusted list GUI
- [CONFIG_CARD_EDGE_CASES.md](CONFIG_CARD_EDGE_CASES.md) — Config card paste edge cases
- [UPGRADE_SYSTEM.md](UPGRADE_SYSTEM.md) — Speed/energy/gas upgrades
- [TIER_UPGRADING.md](TIER_UPGRADING.md) — Machine tier installers and preservation
- [FACTORIES.md](FACTORIES.md) — Parallel processing slots, auto-sort, shared tanks
- [MULTIBLOCK_FORMATION.md](MULTIBLOCK_FORMATION.md) — Formation protocol
- [MULTIBLOCK_INVALIDATION.md](MULTIBLOCK_INVALIDATION.md) — Invalidation and reformation
- [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md) — Canonical rules for Dynamic Tank, Evaporation, Boiler, SPS, Induction Matrix, Turbine, Fission, and Fusion
- [MULTIBLOCK_SPECS.md](MULTIBLOCK_SPECS.md) — Validation sheet, tick order, and worked examples for those structures

## Content Systems
- [MATERIAL_PROGRESSION.md](MATERIAL_PROGRESSION.md) — Ores, alloys, and tier progression
- [ORE_PROCESSING_CHAIN.md](ORE_PROCESSING_CHAIN.md) — 2× to 5× chemical ore processing
- [GENERATORS_REACTORS.md](GENERATORS_REACTORS.md) — Generators, Turbine, Fission, Fusion
- [QIO_SYSTEM.md](QIO_SYSTEM.md) — Frequency-based storage
- [GEAR_MODULE_SYSTEM.md](GEAR_MODULE_SYSTEM.md) — Modular gear and modules
- [RADIATION_SYSTEM.md](RADIATION_SYSTEM.md) — Exposure and protection
- [RADIATION_HUD.md](RADIATION_HUD.md) — Player radiation HUD
- [INDUCTION_MATRIX.md](INDUCTION_MATRIX.md) — Large-scale energy storage design
- [GADGETS.md](GADGETS.md) — Portable tools and gadgets
- [WORLD_GENERATION.md](WORLD_GENERATION.md) — Ore generation rules

## GUI Documentation
- [GUI_LAYOUTS.md](GUI_LAYOUTS.md) — Screen sizes, slot positions, and required widgets
- [GUI_DOCUMENTATION.md](GUI_DOCUMENTATION.md) — Overall GUI patterns and conventions
- [GUI_PATTERNS.md](GUI_PATTERNS.md) — Shared GUI conventions
- [MACHINE_GUI.md](MACHINE_GUI.md) — Standard processing machine GUI
- [MACHINE_IO.md](MACHINE_IO.md) — Slot counts and default sides
- [FACTORY_GUI.md](FACTORY_GUI.md) — Factory parallel slot GUI
- [TANK_GUI.md](TANK_GUI.md) — Fluid and chemical tank GUI
- [ENERGY_CORE_GUI.md](ENERGY_CORE_GUI.md) — Energy Cube / energy storage GUI
- [ENERGY_CELL_GUI.md](ENERGY_CELL_GUI.md) — Energy Cell GUI
- [CHARGE_PAD_GUI.md](CHARGE_PAD_GUI.md) — Charge Pad GUI
- [INDUCTION_MATRIX_GUI.md](INDUCTION_MATRIX_GUI.md) — Induction Matrix GUI
- [MULTIBLOCK_DATA_GUI.md](MULTIBLOCK_DATA_GUI.md) — Multiblock data overview GUI
- [TURBINE_GUI.md](TURBINE_GUI.md) — Industrial Turbine GUI
- [BOILER_EVAP_SPS_GUI.md](BOILER_EVAP_SPS_GUI.md) — Boiler, Evaporation, and SPS GUIs
- [REACTOR_GUI.md](REACTOR_GUI.md) — Fission and Fusion reactor GUIs
- [QIO_GUI.md](QIO_GUI.md) — QIO Dashboard GUI
- [GEAR_GUI.md](GEAR_GUI.md) — Gear and Modification Station GUI
- [CONFIGURATOR_GUI.md](CONFIGURATOR_GUI.md) — Configurator tool modes

## Process and Support
- [FRAMEWORK_IMPLEMENTATION_ORDER.md](FRAMEWORK_IMPLEMENTATION_ORDER.md) — Coding sequence (canonical)
- [FRAMEWORK_ORDER.md](FRAMEWORK_ORDER.md) — Earlier framework notes
- [FRAMEWORK_DIAGRAM.md](FRAMEWORK_DIAGRAM.md) — Architecture diagram
- [IMPLEMENTATION_GUIDANCE.md](IMPLEMENTATION_GUIDANCE.md) — Practical design notes
- [TESTING_STRATEGY.md](TESTING_STRATEGY.md) — Unit and manual testing
- [DEBUG_TOOL_SPEC.md](DEBUG_TOOL_SPEC.md) — Network reader outputs
- [FAILURE_MODES.md](FAILURE_MODES.md) — Player-facing failure states
- [POWER_BUDGET.md](POWER_BUDGET.md) — Progression energy targets
- [BALANCE_WORKFLOW.md](BALANCE_WORKFLOW.md) — Design notes to data mapping
- [CONFIG_STRUCTURE.md](CONFIG_STRUCTURE.md) — Config file organization and keys
- [PUBLIC_API_SUMMARY.md](PUBLIC_API_SUMMARY.md) — Stable interfaces
- [SYNC_STRATEGY.md](SYNC_STRATEGY.md) — Client/server sync rules
- [PERFORMANCE_GUIDELINES.md](PERFORMANCE_GUIDELINES.md) — Tick and network performance rules
- [NBT_CONVENTIONS.md](NBT_CONVENTIONS.md) — Data component and NBT standards
- [RECIPE_DATA_GENERATION.md](RECIPE_DATA_GENERATION.md) — Recipes and data generation (canonical)
- [CRAFTING_RECIPES.md](CRAFTING_RECIPES.md) — Shaped grids, casings, and the ethylene reaction
- [RECIPE_DATA_GEN.md](RECIPE_DATA_GEN.md) — Earlier recipe notes
- [JEI_EMI_INTEGRATION.md](JEI_EMI_INTEGRATION.md) — JEI and EMI integration
- [SOUNDS_AND_PARTICLES.md](SOUNDS_AND_PARTICLES.md) — Sound and particle event ids
- [LOCALIZATION.md](LOCALIZATION.md) — Language keys and the English inventory
- [TEXTURE_HANDOFF.md](TEXTURE_HANDOFF.md) / [TEXTURE_HANDOFF_EXPANDED.md](TEXTURE_HANDOFF_EXPANDED.md) — Texture paths and the asset list
- [OVERSIZED_BLOCKS.md](OVERSIZED_BLOCKS.md) — Blocks that draw or claim space outside one cube
- [CLEAN_ROOM_ANALYSIS.md](CLEAN_ROOM_ANALYSIS.md) — Architectural learnings
- [ADDITIONAL_SYSTEMS.md](ADDITIONAL_SYSTEMS.md) — Miscellaneous notes
- [PLAN.md](PLAN.md) — Overall project plan

All documents are clean-room and intended for implementation by Grok with design support from Claude.
