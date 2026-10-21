# Gear and Module System

## Goals
- Modular MekaSuit and Meka-Tool with installable modules.
- Shared energy system with machines.
- Clear installation, removal, and conflict rules.

## Core Concepts
- Modules grant capabilities (flight, mining modes, vision, radiation protection, etc.).
- Energy draw for active modules, scaled by activity.
- Installation via Modification Station or suit GUI.
- Modules can have tiers or levels with data-driven costs.
- Conflict resolution for mutually exclusive modules.

## Integration
- Energy drawn from suit inventory or shared pool.
- Radiation protection modules interact with the radiation system.
- Configurator or GUI for module management and reordering.
- Persistence in item data components or NBT.
- HUD elements for active modules and energy.

## Edge Cases
- Module conflicts during installation.
- Energy depletion while flying or mining.
- Module removal while active.
- Multiplayer ownership of gear.
