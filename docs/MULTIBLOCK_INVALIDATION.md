# Multiblock Invalidation and Reformation Protocol — Clean-Room

## Overview
Structures must detect breakage, clean up state, and re-attempt formation efficiently without thrashing.

## Key Patterns
- **Request-driven invalidation**: Block break, neighbor change, or internal state change marks the structure dirty.
- **Rate-limited revalidation**: Do not re-run full formation every tick. Use the same rate limit as formation (e.g., every 5–10 ticks).
- **Fail-fast on known invalid**: If a frame already failed this tick, skip further work.
- **Internal vs frame distinction**: Changes to pure internal components (e.g., coil activation) can often skip full revalidation. Frame or controller changes force full recheck.
- **Cache cleanup**: On invalidation, clear MultiblockData, reset bounding blocks, and update the manager cache.
- **Merging awareness**: When adjacent structures exist, prefer merging into the larger valid structure.

## Flow
1. Event marks structure dirty.
2. On next allowed tick, run validation.
3. If invalid → reset structure, clear data, notify clients.
4. If valid → update data if needed or reform.
5. Cooldown after failed attempts to prevent spam.

## Lessons for Mekanika
- Keep invalidation cheap and request-based.
- Separate "internal state change" from "structure change".
- Always provide clear player feedback on why a structure broke.
