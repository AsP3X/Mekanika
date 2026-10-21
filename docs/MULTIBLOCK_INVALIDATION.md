# Multiblock Invalidation and Reformation Protocol — Clean-Room

## Overview
Structures must detect breakage, clean up state, and re-attempt formation efficiently without thrashing.

## Key Patterns
- **Request-driven invalidation**: Block break, neighbor change, or internal state change marks the structure dirty.
- **Rate-limited revalidation**: Do not re-run full formation every tick. Use the 10-tick formation period from [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md).
- **Fail-fast on known invalid**: If a frame already failed this tick, skip further work.
- **Internal vs frame distinction**: Replacing, adding, or removing an interior block marks the structure dirty for a full recheck, because cells, coils, blades, and fuel change the aggregated rules. A state change that does not change the block id can update aggregated stats without a new flood fill. Frame or controller changes force a full recheck.
- **Cache cleanup**: On invalidation, clear formed links, reset bounding blocks, and update the manager cache. Tank contents and stored energy stay on the controller.
- **One shell, one controller**: A connected shell forms only with exactly one controller. Contents stay on that controller. The procedure is in [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md).

## Flow
1. Event marks structure dirty.
2. On next allowed tick, run validation.
3. If invalid → clear formed links and aggregated cache, keep controller tank and energy values, notify clients.
4. If valid → update data if needed or reform.
5. The 10-tick period is the cooldown between failed attempts.

## Lessons for Mekanika
- Keep invalidation cheap and request-based.
- Separate "internal state change" from "structure change".
- Always provide clear player feedback on why a structure broke.
