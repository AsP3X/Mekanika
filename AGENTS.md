# AGENTS.md — Binding Rules for Mekanika

These rules are mandatory for all agents (Grok coding, Claude design/textures/support).

Adapted from AsP3X/AIRuleBook (agent-non-negotiables, scope-discipline, git-commits, root-cause-debugging, definition-of-done, codebase-conventions, dependency-safety, no-deprecated-apis, inline-documentation) and Mekanika project decisions.

## Core Non-Negotiables

- Treat every rule in this file as a **binding requirement**. When rules conflict, follow the **most restrictive** reading and note the conflict.
- **Scope discipline**: Change only what the current task requires. No drive-by refactors, renames, formatting, or unrequested features. Report out-of-scope findings under "Noticed, not changed".
- **Git**: Commit, push, merge, or open PRs **only** on explicit user request. Use exact prefixes: `TASK:`, `FIX:`, `BUGFIX:`, `DOCS:`, `CHORE:`. No AI attribution (no `Co-Authored-By` or "Generated with" footers). Stage only relevant files. One logical change per commit. Never `git add .` unless explicitly asked. Never bypass hooks with `--no-verify` unless asked.
- **Root-cause debugging**: Reproduce and explain the root cause before changing code. No symptom patches (silent catches, increased timeouts, disabling checks). One hypothesis at a time. Guard fixes with tests.
- **Definition of Done**: Evidence required before claiming complete (builds clean, tests pass where applicable, lint no new suppressions, docs updated, no secrets, assets integrated or path documented). State the evidence.
- **No secrets** in code, commits, logs, or output.
- **Dependencies**: Ask before adding or major-upgrading. Prefer existing or standard library. Verify the exact package. Pin versions. Check licenses. Report changes.
- **No deprecated APIs**. Replace or reimplement. Do not suppress deprecation warnings to leave old calls.
- **Inline documentation**: Non-trivial code gets clear comments explaining *why*. Prefer paired `// Human:` + `// Agent:` style where useful for complex logic.
- **Codebase conventions**: Follow existing patterns. Reuse before writing. Keep directory structure. Ask before new top-level folders or dependencies.
- **Never fabricate** file paths, APIs, registry names, or results. State uncertainty.
- **Plans are binding**: Treat agreed phases and plans as checklists. Do not silently skip major steps.

## Project-Specific Rules (Mekanika)

- **Textures (Claude only)**: All block textures created exclusively by Claude. Exactly **16×16 pixels**. Original designs matching industrial style (tier colors: Basic=green, Advanced=red, Elite=cyan, Ultimate=purple) but **not** exact copies of Mekanism blocks. Deliver PNGs + handoff checklist.
- **Architecture**: Architectury multi-loader. Common module holds nearly all logic. Platform modules are thin (entrypoints, energy adapters). Generators, Storage, and Tools depend only on Core + PowerAPI (no circular dependencies).
- **PowerAPI**: Consistent energy unit (Joules, long-based) and transfer model across loaders. Full custom on Fabric; bridges or shared on NeoForge/Forge. Document the model early.
- **Data-driven**: Prefer configurable values and data-driven recipes (JSON / data generators) for rates, capacities, processing speeds. Claude’s balance notes feed into data files rather than hard-coded values.
- **Multiblocks & Networks**: Robust validation. Efficient tick and network logic. Clear failure modes and player feedback for radiation, fission, and fusion defined early. Use appropriate logging levels; surface meaningful failures to the player (not just console).
- **Branding**: Unique. No trademarked names or exact assets from other mods.
- **License**: Respect the repository owner's chosen license. Do not invent or silently add license files.
- **Debug support**: Lightweight debug command or config flag for energy networks and multiblock formation added early.

## Minecraft Implementation Notes

- **Java style**: Clear, readable modern Java (JDK 25 appropriate for MC 26.2). Naming: `camelCase` methods/fields, `PascalCase` classes, `UPPER_SNAKE_CASE` constants.
- **BlockEntities & logic**: Separate sided logic carefully. Prefer capabilities or lookup systems for energy, chemicals, and inventories. Keep tick methods efficient.
- **Common pitfalls**: Avoid heavy work on the client thread; handle NBT sync properly; validate multiblock structures before running logic; watch for performance in large networks.
- **Testing bar (minimum)**: Energy networks transfer correctly under load; multiblock formation and disassembly work and reject invalid structures; chemical processing and ore multiplication produce expected outputs; basic regression tests for changes to core systems.

## Texture Handoff Checklist (Claude → Grok)

1. Filename matching registry ID.
2. Exactly 16×16 PNG.
3. Expected block/item ID.
4. Any animation frames or special states.
5. GUI or particle references if applicable.
6. Style notes (tier color, material).
7. Suggested path (e.g. `assets/mekanika/textures/block/`).

## Shared Workflow

- State a short plan before large changes.
- Prefer small, verifiable steps.
- When blocked, stop and report clearly instead of guessing.
- All visual assets come from Claude; Grok integrates them.
- Out-of-scope findings are reported, not fixed in the current change.
