# Factory Runtime

Parallel slots for the twelve processing machines. Counts, joules, and the screen rows are already fixed. This file is the tick. A factory uses the same tanks and the same side defaults as its base machine, from [MACHINE_IO.md](MACHINE_IO.md). Pixel positions are in [GUI_LAYOUTS.md](GUI_LAYOUTS.md).

The public factory keeps a recipe cache per slot and sorts an inventory only after it changes. Mekanika uses that split. Tanks and upgrades are shared. Recipes are not.

## Normative constants

```normative
factory.slots.basic=3
factory.slots.advanced=5
factory.slots.elite=7
factory.slots.ultimate=9
factory.auto_sort.default=0
factory.buffer_ticks=20
factory.stack_limit=64
factory.sort.on_change=1
```

Basic enrichment draws 1,000 J/t per active slot. Three active slots draw 3,000 J/t. The energy buffer is the base machine's buffer: `joules_per_tick * 20`. An enrichment factory holds 20,000 J, the same as one enrichment chamber.

## Slots

Each factory slot has its own input items, output items, and progress. A machine with no item slots still has one progress counter per factory slot. Those counters share the chemical and fluid tanks.

A slot looks up a recipe from its own input. Another slot may hold a different item and a different recipe on the same tick. A slot whose input matches no recipe draws 0 J and stays at progress 0.

Progress is saved as `mekanika:slot_progress`, a list of ints, one per slot. A missing entry is 0. The single `mekanika:progress` key on a factory stays 0. `mekanika:auto_sort` is a bool. Missing means off.

A tier installer appends new slots at progress 0 and keeps the existing list in index order.

## Shared contents

One energy buffer. One set of chemical tanks. One set of fluid tanks. One set of eight upgrade slots. One side configuration. One redstone mode. Speed, energy, and gas upgrades are computed once and applied to every slot. Eight speed upgrades make every enrichment slot 22 ticks at 9,000 J/t. Three active slots then draw 27,000 J/t.

## Tick

Redstone is checked once. When it disallows running, no slot increments and the block status is `redstone_off`.

Otherwise, in order:

1. Auto-sort, when the rule below says it runs.
2. Slots from index 0 upward. Each slot sees the energy and the tanks left by the earlier slots.

A slot increments by 1 when all of these are true:

- Its input matches a recipe, and the input count is at least the recipe count.
- The output can accept the result.
- The energy buffer holds at least this slot's joules per tick. The slot then removes that many joules.
- If this increment would finish the batch, the shared tanks hold the chemical and fluid cost. The slot pays that cost as it finishes. A flow machine pays the per-tick cost instead, and skips the increment when the tanks cannot pay.

A slot that finishes sets its progress to 0 and emits the result. A slot that cannot increment leaves its progress unchanged. Later slots still run.

A basic enrichment factory with 2,500 J stored and three slots ready costs 1,000 J each. Slot 0 takes 1,000. Slot 1 takes 1,000. Slot 2 sees 500 and does not increment. The buffer ends at 500.

An infuser batch is 200 ticks and costs 20 mB of redstone infusion when it finishes. Two slots sit at progress 199. The shared tank holds 20 mB. Slot 0 finishes, pays 20, and returns to 0. Slot 1 stays at 199.

If the player changes a slot's item id while its progress is above 0, that progress becomes 0 before sort and before the slot runs. Other slots keep their progress.

A full output blocks only that slot. The next slot still increments.

Block status is `running` when any slot incremented. It is `redstone_off` when redstone stopped the block. Otherwise it is `idle`. A slot tooltip uses `running`, `missing_input`, or `output_full`.

## Auto-sort

The button at (120, 4) toggles the flag. Default is off. A config card copies the flag and does not copy `mekanika:slot_progress`.

Sort runs at step 1 only when the flag is on and the input slots changed since the last sort. Turning the flag on counts as a change. A tick with no input change moves 0 items. Outputs, upgrades, and the tanks are not sorted.

1. A slot with progress above 0 is pinned. Its id and count stay. It is not gathered and it does not receive a different id.
2. If every unpinned slot is empty, stop.
3. If the unpinned slots contain more distinct item ids than there are unpinned slots, stop. Nothing moves.
4. Gather the unpinned stacks by item id. Those slots become empty.
5. Walk the ids in lexicographic order. Give each id one empty slot, lowest index first.
6. While an empty slot remains and some id still has `count > assigned slots`, give the next empty slot to the id with the largest count. Equal counts go to the lexicographically smaller id.
7. Split each id across its assigned slots. `share = count / n` and `extra = count % n`, integer division. Lower indexes receive the extra 1 first. No slot receives more than 64. If the split cannot place every item without passing 64, restore the unpinned slots and move 0.

Ten of one ore in slot 0 of a basic factory, progress 0, sorts to 4, 3, 3.

Slot 0 holds 5 copper with progress 40. Slot 1 holds 6 tin. Slot 2 is empty. Copper stays at 5. Tin sorts across the two unpinned slots as 3 and 3.

Slot 0 holds 8 `minecraft:iron_ore`. Slot 1 holds 4 `minecraft:gold_ore`. Slot 2 is empty. Gold is lexicographically first and receives slot 0. Iron has the larger count and receives slots 1 and 2. The result is 4 gold, 4 iron, 4 iron.

A slot holds one item id, so three slots hold at most three ids. The guard still applies when a caller presents more ids than unpinned slots: four ids and three slots move 0.

A washer has no item inputs. Sort moves 0.

## Worked-example index

```normative-examples
example.slots.basic=3
example.draw.all3=3000
example.draw.one=1000
example.buffer=20000
example.energy.ran=2
example.energy.left=500
example.speed8.ticks=22
example.speed8.per_slot=9000
example.speed8.factory3=27000
example.sort.even0=4
example.sort.even1=3
example.sort.even2=3
example.sort.pinned=5
example.sort.tin0=3
example.sort.tin1=3
example.sort.gold=4
example.sort.iron0=4
example.sort.iron1=4
example.sort.too_many=0
example.sort.idle_tick=0
example.sort.washer=0
example.chem.slot0=0
example.chem.slot1=199
example.chem.left=0
example.output.slot0_delta=0
example.output.slot1_delta=1
example.mismatch.draw=0
example.progress.reset=0
```
