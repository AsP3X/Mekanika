# Chemical System Contract — Clean-Room

## Core Concepts
- **Chemical**: Immutable type (gas, infusion, pigment, slurry).
- **ChemicalStack**: Amount + type. Treat as immutable when returned from handlers.
- **IChemicalTank**: Capacity, insert, extract, set, onContentsChanged.
- **IChemicalHandler**: Multi-tank interface with sided support.
- **Merged tanks**: Some machines support multiple chemical types but only one at a time.

## Tanks
- Tiered capacity and output rate (Basic doubles to Ultimate).
- Auto-eject, dump modes.
- Input/output item slots for filling/draining tanks from items.

## Networks
- Pressurized Tubes form chemical networks with capacity and transfer rate.
- Pull/push modes and side config apply.

## Lessons for Mekanika
- Define Chemical, ChemicalStack, IChemicalTank, and IChemicalHandler early.
- Support sided handlers from the start.
- Keep chemical inventories separate from fluids and items.
- Document unit (mB) and transfer rules clearly.
