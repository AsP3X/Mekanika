# Public API

Stable types for addons. Package `mekanika.api`. Concrete block entities stay in the implementation package. A method that says it returns a copy never returns its internal object.

Amounts are `long`. Energy is joules. Chemicals and fluids are millibuckets. A negative amount throws `IllegalArgumentException`. A null argument throws `NullPointerException`.

## Normative constants

```normative
api.version=1
api.empty_amount=0
api.steam_default_deci_c=1000
api.progress_scale=100
```

## Action

`SIMULATE` reports the result and changes nothing. `EXECUTE` applies it. Both return the amount moved, which may be less than the request.

## Energy

`IEnergyContainer`

| Method | Contract |
| --- | --- |
| `long getEnergy()` | Stored joules, `0` through `getCapacity()` |
| `long getCapacity()` | At least 0 |
| `long getNeeded()` | `getCapacity() - getEnergy()` |
| `long insert(long amount, Action action)` | Accepted joules, never more than `getNeeded()` |
| `long extract(long amount, Action action)` | Removed joules, never more than `getEnergy()` |
| `boolean isEmpty()` | `getEnergy() == 0` |

`IEnergyHandler`

| Method | Contract |
| --- | --- |
| `int size()` | Container count |
| `IEnergyContainer get(int index)` | Index from 0 inclusive to `size()` exclusive |
| `IEnergyContainer get(Direction side)` | Null when that side does not expose energy |

`EnergyText.format(long joules)` returns a short string with an SI prefix. 1,000 is `1 kJ`. 1,000,000 is `1 MJ`.

## Chemicals

`ChemicalStack` holds a chemical id and a `long` amount. `empty()` is the only empty stack. `isEmpty()` is true when the amount is 0. `copy()` is a new stack. Callers do not mutate a stack they did not create.

Steam may carry `temperature_deci_c`. A missing value means 1,000, which is 100.0°C. Merging two steam amounts uses the weighted integer rule in [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md).

`IChemicalTank` matches `IEnergyContainer` with these extras:

| Method | Contract |
| --- | --- |
| `ChemicalStack getStack()` | Copy of the contents |
| `boolean isValid(Chemical id)` | False rejects that chemical |
| `long insert(ChemicalStack stack, Action action)` | Wrong type into a non-empty tank accepts 0 |
| `ChemicalStack extract(long amount, Action action)` | A new stack, or empty |

`IChemicalHandler.get(int)` and `get(Direction)` follow the energy handler. A side mode of none returns null.

## Fluids

Fluids use the loader's fluid handler. Mekanika does not publish a second one. `IFluidView` adds `long getAmountMb()` and `long getCapacityMb()` for GUIs. One bucket is 1,000 mB.

## Sides

`Transmission`: `ITEM`, `FLUID`, `CHEMICAL`, `ENERGY`, `HEAT`.

`SideMode`: `NONE`, `INPUT`, `OUTPUT`, `BOTH`. Used by machines and ports.

`FaceMode`: `NORMAL`, `PUSH`, `PULL`, `NONE`. Used by transmitters. The table is in [TRANSMITTER_CONFIG.md](TRANSMITTER_CONFIG.md).

`ISideConfig.get(Transmission, Direction)` and `set` read and write one mode. `set` on a type the block does not support throws `IllegalArgumentException` and changes nothing.

## Multiblocks

`FormationResult` is `ok`, a `FormationCode`, and an optional position. The codes are the failure table in [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md).

`IMultiblockView`

| Method | Contract |
| --- | --- |
| `boolean isFormed()` | |
| `BlockPos min()` / `max()` | Inclusive bounds when formed. Throws `IllegalStateException` when not formed |
| `FormationCode lastFailure()` | Null when the last attempt succeeded |

Addon code can read this view. It cannot mark a structure formed.

## Gear modules

`IModuleType` exposes `id`, `maxLevel`, and `conflicts`. `ModuleRegistry.register` adds one type. Registering the same id twice throws. A module's piece restrictions and energy costs are the table in [GEAR_MODULE_SYSTEM.md](GEAR_MODULE_SYSTEM.md).

## Computers

If no computer mod is loaded, these methods are not registered. A call from a player who fails the block's security check throws the text `You cannot use this frequency.` only for QIO. Every other block throws `Access denied.`

| Method | Returns |
| --- | --- |
| `getEnergy()` | long joules |
| `getCapacity()` | long joules |
| `getProgress()` | int, 0 through 100 |
| `getStatus()` | string status from that block's doc |
| `getChemical(int tank)` | id and millibuckets |
| `getRadiation()` | sieverts per hour at the block |
| `scram()` | fission or boiler only. Other blocks throw `Unsupported.` |

`scram()` on a fission reactor sets the scram flag from the structure spec. It does not bypass a security failure.

## Worked-example index

```normative-examples
example.insert.simulate_unchanged=true
example.insert.over_capacity=0
example.steam.missing_temperature=1000
example.energy_text.1000=1 kJ
example.progress.max=100
```
