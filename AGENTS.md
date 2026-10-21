# Agent Rules for Mekanika

These rules are binding for Grok (coding) and Claude (design, textures, documentation).

Adapted from AsP3X project practices (epochAI AGENTS.md, shroud .cursor/rules, custom licenses).

## Core Non-Negotiable Rules

1. **No commits, pushes, or PRs without explicit user permission.**
2. **License awareness**: Do not assume open-source freedom. Never invent or silently add a LICENSE. Mark third-party snippets.
3. **Definition of Done**: Feature works/tested, clean code, relevant docs/comments, assets integrated or path documented, clear status to user.
4. **Touch only what is required**. No drive-by refactors.
5. **Never fabricate** file paths, APIs, or results.
6. **Plans are binding**. Treat agreed phases as checklists.

## Rules for Grok (Coding)

- Prefer clear, readable modern Java (JDK 25 appropriate for MC 26.2).
- Naming: camelCase methods/fields, PascalCase classes, UPPER_SNAKE_CASE constants.
- Keep as much logic as possible in the common (Architectury) module.
- Platform-specific code minimal and separated.
- PowerAPI: full custom on Fabric; clean bridges or shared on Forge/NeoForge.
- Data-driven recipes and configs where practical. Avoid magic numbers.
- Validate all network packets and player interactions. Do not trust client data.
- Write or describe tests for critical systems (energy, multiblocks, chemicals).
- Non-trivial logic gets clear "why" comments.
- Clear modular structure matching PowerAPI / Core / Generators / Storage / Tools.

## Rules for Claude (Design & Support)

- **Textures**: Exactly 16×16 pixels. Mekanism-inspired industrial aesthetic but **original designs**. Tier colors: green/red/cyan/purple.
- Deliver individual PNGs ready for resource pack structure + handoff notes.
- Maintain coherent visual language across all blocks.
- Produce design documents, recipe trees, exact multiblock blueprints, balance notes.
- Stick to agreed scope. Suggest additions only when asked.

## Shared Workflow

- State a short plan before large changes.
- Prefer small, verifiable steps.
- When blocked, stop and report clearly.
- All visual assets come from Claude; Grok integrates them.
- Early lightweight debug command/config for energy networks and multiblock formation.
- Document PowerAPI energy unit and transfer model clearly.
- Define clear failure modes early for radiation/fission/fusion.

## Texture Handoff Checklist

For each texture Claude produces:
- Filename and expected path
- Corresponding block/item ID
- 16×16 confirmation
- Animation frames (if any)
- GUI or special notes

See also `docs/TEXTURE_HANDOFF.md`.
