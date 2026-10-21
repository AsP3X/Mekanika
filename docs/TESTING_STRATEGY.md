# Testing Strategy

## Unit Tests
- Multiblock validators with synthetic structures.
- Tank insert/extract with simulation mode.
- Network merge and split logic.
- Side configuration mode cycling.
- Upgrade effect calculations.

## Integration Tests
- Full ore processing chain (2× to 5×).
- Energy network throughput under load.
- Chemical network filtering and distribution.
- Multiblock formation and invalidation cycles.
- Config card copy/paste edge cases.

## Manual Testing
- Formation of small and maximum-size structures.
- Factory parallel processing and auto-sort.
- Radiation exposure and protection.
- Gear module installation and energy draw.
- Multi-loader verification (Forge, Fabric, NeoForge).

## Regression
- Add tests for any fixed formation, network, or sync bugs.
- Document expected failure modes and verify player feedback.

## Notes
- Prefer headless unit tests for logic.
- Use creative mode or debug tools for manual verification.
- Track performance on large networks and max-size multiblocks.
