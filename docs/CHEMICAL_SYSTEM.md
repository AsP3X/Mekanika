# Chemical System Contract — Clean-Room

This document defines the conceptual design for Mekanika’s chemical system. All content is original and based on observed public patterns. No code or assets are reproduced.

## Core Concepts

- **Chemical**: Immutable type identifier. Categories include gas, infusion, pigment, and slurry. Each chemical has attributes (e.g., radioactive, gaseous, liquid-like).
- **ChemicalStack**: Type + amount (long, in mB). Stacks returned from handlers must not be mutated by callers. Empty stack is a singleton or clearly identifiable.
- **IChemicalTank**: Single tank interface.
  - Capacity (long).
  - Current amount and type.
  - insert / extract (with Action: EXECUTE or SIMULATE).
  - set / setEmpty.
  - onContentsChanged callback.
  - isValid / isTypeEqual.
  - Optional filtering.
- **IChemicalHandler**: Multi-tank interface with sided support.
  - getChemicalTanks().
  - getChemicalInTank(int).
  - insertChemical / extractChemical (with side awareness).
  - setChemicalInTank (restricted use).
- **Merged tanks**: Some machines (e.g., crystallizer, dissolution chamber) support multiple chemical types but only one type may be present at a time. Insertion into a non-empty mismatched tank is rejected.

## Tank Features

- Tiered storage: Basic, Advanced, Elite, Ultimate. Capacities are in [POWER_BUDGET.md](POWER_BUDGET.md). Per-tick rates are 250, 4,000, 32,000, and 64,000 mB, from [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md).
- No creative chemical tank is registered.
- Auto-eject flag (pushes to adjacent tubes or machines).
- Dump modes:
  - None
  - Dump excess
  - Dump all (continuous drain)
- Input and output item slots for filling/draining tanks from portable containers.
- Side configuration support (input / output / none per face).
- Redstone control and security integration.

## Networks

Pressurized tubes use the per-block buffer and the fair split in [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md). A tube holds one chemical id. A different id does not connect, and neither buffer is deleted. Throughput is 250, 1,000, 4,000, and 16,000 mB/t. Buffers are 4,000, 16,000, 256,000, and 1,024,000 mB.

## Production and Consumption Patterns

- Machines declare input and output chemical tanks.
- Recipe system specifies chemical inputs/outputs with amounts.
- Common production loops:
  - Water electrolysis → `mekanika:hydrogen` + `mekanika:oxygen`.
  - Brine processing → chlorine / hydrogen chloride.
  - Sulfuric acid production for 5× ore processing.
  - Slurry → clean slurry → crystal intermediates.
- Chemical consumption is per-tick or per-operation, scaled by speed upgrades.
- A factory has one shared chemical tank set. Each slot pays that tank in index order, from [FACTORIES.md](FACTORIES.md).

## Integration Points

- **Machines**: Enrichment, dissolution, crystallizer, infuser, oxidizer, washer, etc., all use the same tank and handler interfaces.
- **Transmitters**: Pressurized Tubes use the chemical network system.
- **Storage**: Chemical Tanks and Dynamic Tanks (multiblock) use the same tank contracts.
- **Gear**: Optional modules that interact with chemicals (e.g., radiation shielding related to radioactive chemicals).
- **JEI/EMI**: Display chemical inputs/outputs and tank capacities.
- **Data generation**: Recipes and tags for chemicals are data-driven.

## Implementation Recommendations for Mekanika

1. Define Chemical, ChemicalStack, IChemicalTank, and IChemicalHandler interfaces early in the API.
2. Implement a basic tank class that handles capacity, insertion, extraction, and change notifications.
3. Support sided handlers from the start so side config works immediately.
4. Keep chemical inventories completely separate from fluid and item systems.
5. Document the unit (mB) and any conversion rules clearly in the PowerAPI/Chemical design docs.
6. Add dump modes and auto-eject early so players can manage overflow.
7. Provide a debug command or network reader that shows chemical network contents and tank levels.
8. Design for mixed-type support where needed (merged tanks) but prefer single-type networks for simplicity and performance.

## Edge Cases to Consider

- Empty vs. partially filled tanks during network merges.
- Rapid insertion/extraction that exceeds throughput.
- Creative tanks interacting with finite networks.
- Radioactive chemicals and player exposure (if radiation system is implemented).
- Chunk unload while chemicals are in tubes or tanks.
- Config card copying of chemical-related side configs.

This contract should allow machines, tubes, tanks, and multiblocks to share a consistent chemical system without tight coupling.
