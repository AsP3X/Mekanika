# Item Transport

Logistical transporters move item stacks along a path. The public Mekanism pathfinder is the pattern: each block has a cost, a stack keeps the color it entered with, and a destination is used only when that inventory can accept the items. Speeds and pull sizes are the public tier numbers. Buffered cables, tubes, pipes, and conductors are not this system. Their link rule is in [TRANSMITTER_CONFIG.md](TRANSMITTER_CONFIG.md).

## Normative constants

```normative
transport.progress_per_block=100
transport.basic.speed=5
transport.basic.pull=1
transport.basic.cost=10
transport.advanced.speed=10
transport.advanced.pull=16
transport.advanced.cost=5
transport.elite.speed=20
transport.elite.pull=32
transport.elite.cost=2
transport.ultimate.speed=50
transport.ultimate.pull=64
transport.ultimate.cost=1
transport.restrictive.cost_multiplier=4
transport.pull.success_delay=10
transport.pull.fail_cap=40
transport.idle.recheck_ticks=10
transport.idle.drop_ticks=100
transport.sorter.filters=16
transport.colors=16
transport.search_node_cap=1024
```

Cost is `50 / speed` using integer division. Elite speed 20 costs 2. A restrictive block multiplies its cost by 4, so a basic restrictive block costs 40.

## Blocks

| Id | Role |
| --- | --- |
| `mekanika:logistical_transporter_basic` | Speed 5, pulls 1 item |
| `mekanika:logistical_transporter_advanced` | Speed 10, pulls 16 |
| `mekanika:logistical_transporter_elite` | Speed 20, pulls 32 |
| `mekanika:logistical_transporter_ultimate` | Speed 50, pulls 64 |
| `mekanika:restrictive_transporter` | Basic speed, filter, cost times 4 |
| `mekanika:diversion_transporter` | Basic speed, one front face gated by redstone |
| `mekanika:logistical_sorter` | Pulls 64, up to 16 filters, paints a color onto the stack |

## Color

Sixteen dye colors, plus none. None is the default. Two transporters connect when both are none, either is none, or the colors match. A stack copies the color of the transporter it first enters. A none transporter does not change a stack that already has a color. A later block whose color is set and different from the stack is not a legal step.

Changing a block's color does not recolor stacks already inside it. On the next check, a mismatch drops that stack as an item at the block.

## Path

Search is A* over connected transporters. The step cost is that block's cost. Equal total cost prefers fewer blocks. A destination inventory is legal only when a simulated insert of the stack into the facing side succeeds. The machine's side configuration is that simulation.

The search does not enter an unloaded chunk. It also stops after 1,024 transporter blocks. A stopped or blocked search leaves the stack idle. A stack that reaches the last loaded block waits there and retries every 10 ticks. It is not dropped for an unloaded neighbor.

The chosen path is stored on the stack. Each tick the stack adds the speed of the block it occupies to its progress. At 100 progress it moves to the next block and progress resets. Crossing one basic block takes 20 ticks. Crossing one ultimate block takes 2 ticks.

If the next block is gone or the path fails the color check, the stack becomes idle. An idle stack searches for a new destination for 100 ticks, then for the inventory it came from. If both fail for those 100 ticks, it drops.

A diversion transporter has one front face. While that block is powered, the front may be a path step into an inventory or another transporter. While unpowered, the front is not a step. Other faces stay open.

## Pull, backup, and filters

A face in Pull mode takes up to the tier's pull count from the inventory every time its delay hits 0. A successful pull waits 10 ticks. A failed pull increments a failure count and waits `min(40, 2 ^ failures)` ticks. Success resets the count. Push faces send and do not pull. None faces do not connect. Normal faces connect and do not pull. The default is Normal.

A full destination is not chosen. The items stay at the source. If the source later rejects them too, the stack uses the idle rule and can drop.

A restrictive transporter with an empty filter accepts nothing. With filters, a stack must match one filter to enter. Filters are an item id or a tag. The first match wins.

The sorter pulls 64 items per attempt from the inventory it sits on. Filters are checked in list order, up to 16. Each filter may set a color and a destination side. The first match sends the stack in that color. No match leaves the item in the inventory. When several inventories accept the matched item, the sorter rotates through them and remembers the last one used.

A filter edit does not rewrite a stack already on a path. The next pull uses the new list.

## Chunks and drops

In-transit stacks are saved on the transporter that holds them. A chunk unload leaves them there. They resume when the chunk loads. A broken transporter drops the stacks that were inside it.

## Debug

The reader shows color, tier, face modes, delay, filter count, and, for the stack under the cursor, progress and remaining path length.

## Worked-example index

```normative-examples
example.basic.ticks_per_block=20
example.ultimate.ticks_per_block=2
example.basic.cost=10
example.elite.cost=2
example.four_basic.cost=40
example.restrictive_basic.cost=40
example.ultimate.pull=64
example.pull.success_delay=10
example.pull.fail6_delay=40
example.mixed.three_basic_one_ultimate=31
example.mixed.two_elite=4
example.search_node_cap=1024
```
