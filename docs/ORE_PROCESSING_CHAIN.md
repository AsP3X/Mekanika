# Ore Processing Chain (2× to 5×)

## Goals
- Progressive ore multiplication with increasing complexity and chemical requirements.
- Clear machine and chemical roles at each tier.
- Data-driven ratios and yields.

## Stages

### 2× Processing
- Enrichment Chamber: Ore → Crushed Ore.
- Smelter: Crushed Ore → Ingot (2×).

### 3× Processing
- Purification Chamber: Crushed Ore + Oxygen → Clump.
- Chemical Injection Chamber: Clump + Hydrogen Chloride → Shard.
- Smelter: Shard → Ingot (3×).

### 4× Processing
- Chemical Dissolution Chamber: Shard + Sulfuric Acid → Slurry.
- Chemical Washer: Slurry + Water → Clean Slurry.
- Smelter: Clean Slurry → Ingot (4×).

### 5× Processing
- Chemical Crystallizer: Clean Slurry → Crystal.
- Smelter: Crystal → Ingot (5×).

## Supporting Chemicals
- Hydrogen Chloride, Sulfuric Acid, Oxygen, Water.
- Produced via dedicated machines or multiblocks.

## Integration
- Factories support parallel processing of any stage.
- Side configuration for item and chemical I/O.
- JEI/EMI shows the full chain and intermediate items.
- Ratios and yields are data-driven.

## Notes
- Document expected yields and byproducts.
- Balance lives in data files or configs.
- Debug tool can report chemical consumption rates.
