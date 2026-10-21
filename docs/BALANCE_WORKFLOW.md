# Balance Workflow

## Goals
- Allow balance changes without code edits.
- Map design notes directly to data or config keys.

## Process
1. Claude provides balance notes (rates, ratios, multipliers).
2. Values are placed in config files or data packs.
3. Machines and structures read values at initialization.
4. Changes are versioned with the mod.

## Key Areas
- Tier multipliers (energy, transfer, process speed).
- Chemical and fluid ratios.
- Generator output and efficiency.
- Ore processing yields.

## Notes
- Keep magic numbers out of core logic.
- Document the mapping from design notes to data keys.
