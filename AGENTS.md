# AGENTS.md — Mekanika

Binding rules for all coding and design agents working on this project.

## Core Non-Negotiables

1. **No commits, pushes, or PRs without explicit user permission.**
2. **License awareness**: Do not assume open-source freedom. Follow the owner's chosen license. Mark third-party code clearly.
3. **Definition of Done**: Feature works + tested (or clear test plan), code is clean, documentation updated, assets integrated or path documented, status summary provided.
4. **Touch only what is required**: No drive-by refactors or unsolicited changes.
5. **Never fabricate**: Do not invent paths, APIs, or results. State uncertainty.
6. **Plans are binding**: Treat agreed phases as checklists.

## Grok (Coding) Rules

- Prefer clear, readable modern Java suitable for Minecraft 26.2 / JDK 25.
- Keep logic in the Architectury common module; platform modules stay thin.
- PowerAPI must be consistent across loaders (full custom on Fabric, bridge/shared elsewhere).
- Use data-driven recipes and configs wherever practical.
- Validate network packets and player interactions. Do not trust client data for machine logic.
- Write or describe tests for critical systems (energy, multiblocks, chemicals).
- Non-trivial logic requires clear comments explaining *why*.

## Claude (Design & Support) Rules

- **All block textures must be exactly 16×16 pixels.**
- Style must match Mekanism’s industrial aesthetic (tier colors: Basic green, Advanced red, Elite cyan, Ultimate purple) but **must not copy exact designs**.
- Deliver individual PNGs with clear naming.
- Provide design documents, recipe trees, multiblock blueprints, and balance notes.
- Maintain visual consistency across all blocks and items.

## Shared Workflow

- State a short plan before large changes.
- Prefer small, verifiable steps.
- When blocked, stop and report clearly.
- All visual assets come from Claude; Grok integrates them.

## Project-Specific Additions

- Texture Handoff checklist required for every asset delivery.
- Early debug command/config for energy networks and multiblock formation.
- Short design note documenting PowerAPI energy unit and transfer model.
- Clear failure modes and player feedback defined early for radiation, fission, and fusion.
