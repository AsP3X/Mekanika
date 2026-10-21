# Buffered Networks

Energy cables, pressurized tubes, and mechanical pipes. Each block stores its own buffer. A network is the set of blocks that can reach each other. It does not keep a second copy of the buffer. Item pathfinding stays in [ITEM_TRANSPORT.md](ITEM_TRANSPORT.md). Conductor math stays in [HEAT_SYSTEM.md](HEAT_SYSTEM.md). Face names stay in [TRANSMITTER_CONFIG.md](TRANSMITTER_CONFIG.md).

Public pipes store a buffer that is separate from the amount they move in one tick. Public cables use one number for both. Mekanika keeps that split. Cable throughput is already the power-budget series, so a cable's buffer is that same number. Chemical tube throughput stays the power-budget series. Tube buffers and every fluid number below are the public millibucket figures. `FluidType.BUCKET_VOLUME` on that public tier table is 1,000.

## Normative constants

```normative
buffer.bucket_mb=1000
buffer.energy.equals_throughput=1
buffer.chemical.basic=4000
buffer.chemical.advanced=16000
buffer.chemical.elite=256000
buffer.chemical.ultimate=1024000
buffer.fluid.basic=2000
buffer.fluid.advanced=8000
buffer.fluid.elite=32000
buffer.fluid.ultimate=128000
fluid.pipe.basic.throughput=250
fluid.pipe.advanced.throughput=1000
fluid.pipe.elite.throughput=8000
fluid.pipe.ultimate.throughput=32000
fluid.tank.basic.capacity=32000
fluid.tank.advanced.capacity=64000
fluid.tank.elite.capacity=128000
fluid.tank.ultimate.capacity=256000
fluid.tank.basic.throughput=1000
fluid.tank.advanced.throughput=4000
fluid.tank.elite.throughput=16000
fluid.tank.ultimate.throughput=64000
chemical.tank.basic.throughput=250
chemical.tank.advanced.throughput=4000
chemical.tank.elite.throughput=32000
chemical.tank.ultimate.throughput=64000
```

Energy throughput is 1,000, 8,000, 32,000, and 128,000 J/t from [POWER_BUDGET.md](POWER_BUDGET.md). The cable buffer is that throughput. There is no second config key. Chemical tube throughput is 250, 1,000, 4,000, and 16,000 mB/t from the same file. Chemical tank capacity stays there too.

Chemical tank throughput keeps the public empty time. A public basic tank of 64,000 mB at 1,000 mB/t empties in 64 ticks, and 16,000 / 64 is 250. Advanced empties in 16 ticks (64,000 / 16 = 4,000). Elite empties in 8 ticks (256,000 / 8 = 32,000). Ultimate empties in 16 ticks (1,024,000 / 16 = 64,000).

## Blocks

| Id | Buffer | Per tick |
| --- | --- | --- |
| `mekanika:universal_cable_<tier>` | The tier throughput | That throughput, in joules |
| `mekanika:pressurized_tube_<tier>` | 4,000 / 16,000 / 256,000 / 1,024,000 mB | 250 / 1,000 / 4,000 / 16,000 mB |
| `mekanika:mechanical_pipe_<tier>` | 2,000 / 8,000 / 32,000 / 128,000 mB | 250 / 1,000 / 8,000 / 32,000 mB |
| `mekanika:fluid_tank_<tier>` | 32,000 / 64,000 / 128,000 / 256,000 mB | 1,000 / 4,000 / 16,000 / 64,000 mB |
| `mekanika:chemical_tank_<tier>` | The tank capacity in the power budget | 250 / 4,000 / 32,000 / 64,000 mB |

Tier suffixes are `basic`, `advanced`, `elite`, and `ultimate`. Cables, tubes, pipes, and fluid tanks are 16 block ids. Chemical tanks are the four ids already in the storage table. No creative cable, tube, pipe, or tank is registered. A cable, tube, and pipe do not connect to each other. Heat conductors do not connect to them.

English names follow the chemical tank pattern: "Basic Universal Cable", "Basic Pressurized Tube", "Basic Mechanical Pipe", "Basic Fluid Tank".

## What is stored

A cable saves `mekanika:energy`. A tube saves one `mekanika:chemicals` entry. A pipe saves one `mekanika:fluids` entry. All three save `mekanika:sides`. None of them save a network id, an acceptor list, or a neighbor list. A missing buffer loads as empty. A missing face loads as `normal`.

A fluid tank item and a chemical tank item keep their contents in those same components. An empty tank omits the component. Breaking a tank drops that item. Breaking a cable, tube, or pipe drops the empty block and deletes the buffer that was inside the broken block. Neighbors keep their own buffers. No chat line is sent for that deletion.

## Faces

Transmitter faces use `normal`, `push`, `pull`, and `none`. The default is `normal` on all six sides. `none` does not connect and does not move anything. `normal` and `push` accept an eject and can emit. `pull` does those and also extracts. For energy, chemicals, and fluids, `normal` and `push` follow the same row in [TRANSMITTER_CONFIG.md](TRANSMITTER_CONFIG.md).

Tank and cube faces use `INPUT`, `OUTPUT`, `BOTH`, and `NONE`.

| Block | Up | Down | North, south, west, east |
| --- | --- | --- | --- |
| Fluid tank | `INPUT` | `OUTPUT` | `INPUT` |
| Chemical tank | `INPUT` | `OUTPUT` | `INPUT` |

An energy cube stays `BOTH` on every face, from [MACHINE_IO.md](MACHINE_IO.md). A tank offers on `OUTPUT` and `BOTH`. It accepts on `INPUT` and `BOTH`. While redstone does not allow the tank to run, automated faces move 0. The bucket slots in the next section still run. Redstone mode names are in [CONTROL_SYSTEM.md](CONTROL_SYSTEM.md).

## Connections

Two transmitters connect when they are the same kind, both touching faces are not `none`, and one of these is true:

- Both are cables.
- Both chemical or fluid buffers are empty.
- One of those buffers is empty.
- Both hold the same id.

A hydrogen tube beside an oxygen tube does not connect. Both amounts stay. A water pipe beside a lava pipe does not connect. Both amounts stay. An empty tube connects to a tube that holds hydrogen, and the link rules can move hydrogen into it.

Transfer reads the six neighbors at the start of each transmitter phase. It does not wait for a cached network object. The cache used by the debug count rebuilds on place, break, and face change, at most 8 networks a tick, from [PERFORMANCE_GUIDELINES.md](PERFORMANCE_GUIDELINES.md). A cable that is still waiting for that cache still transfers.

## Tick

The transmitter phase runs after machine and tank eject amounts are known. Inside the phase, every transmitter finishes Accept before any transmitter starts Pull, then Emit, then links. Transmitter order inside Accept, Pull, and Emit is `(y, z, x)`. Amounts update as soon as a transmitter finishes, so an earlier position sees the old neighbor value and a later position sees the new one.

Face order is down, up, north, south, west, east. That is ordinals 0 through 5. Neighbor sort for a split is `(y, z, x)` of the neighbor, then the face ordinal.

**Accept.** Inbound room is `min(throughput, capacity - amount)`. Neighbors that are not transmitters and that offer on `OUTPUT` or `BOTH` are acceptors. A neighbor with a throughput key offers `min(stored, throughput)`. An energy cube offers its stored joules. When the buffer is empty and the offers use more than one chemical or fluid id, only the first id in neighbor sort is eligible. A different id into a non-empty buffer is not an acceptor. The room is then fair-split across the eligible offers. Accepted amounts leave the neighbor and enter the buffer.

**Pull.** Remaining inbound budget is `throughput - accepted this tick`. Each `pull` face whose neighbor is not a transmitter, and whose face allows extraction (`OUTPUT` or `BOTH`, or a machine output), is an acceptor. The limit is `min(neighbor stored, neighbor throughput)` when the neighbor has a throughput, otherwise the stored amount. A wrong id has limit 0. Fair-split the remaining budget. A face that already accepted an eject this tick is still a legal pull face only when budget and space remain. Pull does not reach through another transmitter. Transmitter-to-transmitter movement is the link step.

**Emit.** Outbound budget is `min(throughput, amount)`. Targets are neighbors that are not transmitters and whose face accepts (`INPUT` or `BOTH`). Fair-split the budget. A cube face left on `BOTH` is a target, so half of a full emit can return to that cube. An `OUTPUT` face is not a target.

**Links.** After every emit, each connected pair moves once. Link order is the lower block position, then the higher one, as `(y, z, x)`.

## Fair split

`offer` is the room or the budget. Acceptors with limit 0 are dropped. `remaining` starts at `offer`.

1. `share = remaining / n` and `extra = remaining % n`, integer division, `n` the active count.
2. When `share` is 0, give 1 to each of the first `remaining` acceptors and stop.
3. When any active limit is less than or equal to `share`, give each of those its whole limit, subtract from `remaining`, remove them, and repeat from step 1.
4. Otherwise give `share` to every active acceptor, and give one more to the first `extra` of them. Stop.

A single acceptor receives `min(offer, limit)`.

An offer of 10 to limits 100, 100, and 100, in down, north, up order, gives 4, 3, and 3. An offer of 10 to limits 2, 100, and 100 gives 2, then 4 and 4. An offer of 5 to limits 1, 1, and 1 gives 1, 1, and 1, and 2 stays unsent.

## Links

The link rate is `min(throughputA, throughputB)`. A basic cable against an ultimate cable has link rate 1,000 J/t.

`raw = amountA * capacityB - amountB * capacityA`

When `raw` is positive, A is fuller and `q = raw / (capacityA + capacityB)`. When `raw` is negative, B is fuller and `q = (-raw) / (capacityA + capacityB)`. Integer division. `raw` of 0 moves nothing. `q` of 0 moves nothing, including when `raw` is not 0.

`proposed = min(q, linkRate, sourceAmount)`. The sink keeps `min(proposed, sinkCapacity - sinkAmount)`. The source loses only that accepted amount.

A full basic cable (1,000 / 1,000) beside an empty ultimate cable (0 / 128,000):

`q = (1000 * 128000) / 129000 = 992`

The link moves 992 J. The buffers end at 8 and 992. The next tick has `raw = 8 * 128000 - 992 * 1000 = 32000` and `q = 32000 / 129000 = 0`, so it moves 0.

Two basic cables at 600 and 400 move `q = 100` and finish at 500 and 500.

## Tanks and buckets

A fluid tank has one fill slot and one drain slot, the same slot count as a chemical tank. One bucket is 1,000 mB. The tank item does not place or scoop a fluid on use.

On the tank's server tick, the fill slot runs first. A fluid bucket whose fluid matches the tank, or any fluid bucket when the tank is empty, inserts 1,000 mB when the tank has room. The bucket item becomes an empty bucket in the fill slot. A different fluid inserts 0 and the bucket stays. The drain slot does not run on a tick that inserted.

On a later tick, an empty bucket in the drain slot becomes a filled bucket and the tank loses 1,000 mB when the tank holds at least 1,000 mB. A tank below 1,000 mB does not fill a bucket.

A basic fluid tank holds 32 buckets. The 33rd water bucket stays in the fill slot and the amount stays 32,000.

An installer from [TIER_UPGRADING.md](TIER_UPGRADING.md) changes the tier of a cable, tube, pipe, fluid tank, or chemical tank and keeps the contents. Any amount above the new capacity is deleted. The security owner receives `message.mekanika.transmitter.contents_voided`: "Stored contents exceeded the new capacity and the extra was removed". A block with no security owner logs the deletion and sends no chat. These tiers all grow, so a basic cable that holds 1,000 J and becomes advanced keeps 1,000 J in a buffer of 8,000 J.

## Sync and debug

The delta rule in [SYNC_STRATEGY.md](SYNC_STRATEGY.md) uses the block's buffer capacity. A basic fluid pipe of 2,000 mB has threshold `max(1, 2000 / 100) = 20`. A change of 19 waits. A change of 20 sends.

The debug report appends throughput, buffer capacity, and the connected transmitter count to the energy, chemical, or fluid section. A pipe or tube also appends the content id. Those fields are not writable.

## Traces

A basic cable starts empty. A cube on the down face holds 5,000 J. A machine on the up face accepts joules.

The cube face is `OUTPUT`. Accept takes 1,000. Emit has one target, the machine, and moves 1,000. The cable ends empty. The machine gained 1,000. The cube holds 4,000.

The same cube face is `BOTH`. Accept still takes 1,000. Emit splits 1,000 across the cube and the machine, 500 each. The machine gained 500. The cube holds 4,500.

A basic pipe starts empty. Its other faces are `none`. A basic fluid tank on the down face offers water, capped at 1,000 mB/t. Accept takes `min(250, 1000, 2000) = 250`. Eight ticks fill 2,000. The ninth accepts 0.

An empty basic tube has a hydrogen tank on down and an oxygen tank on up. Both offer 250 mB. Only the first id, hydrogen, is eligible. The tube ends at 250 mB hydrogen. The oxygen tank is unchanged.

A tube that already holds 100 mB hydrogen is offered oxygen. Accepted oxygen is 0. The hydrogen amount stays 100.

A basic fluid tank starts empty. The fill slot holds a water bucket and the drain slot holds an empty bucket. After the tick the tank holds 1,000 and the drain slot is still empty. The next tick the drain slot becomes a water bucket and the tank holds 0.

Breaking a tube that holds 100 mB deletes that 100. The neighbor tube that holds 40 mB still holds 40.

## Worked-example index

```normative-examples
example.link.rate=1000
example.link.moved=992
example.link.basic_left=8
example.link.ultimate_gained=992
example.link.next_tick=0
example.link.equalize_moved=100
example.link.equalize_end=500
example.split.down=4
example.split.north=3
example.split.up=3
example.split.capped.down=2
example.split.capped.north=4
example.split.capped.up=4
example.split.ones.sent=3
example.split.ones.unsent=2
example.cable.output_face=1000
example.cable.both_face=500
example.fluid_pipe.accept=250
example.fluid_pipe.ticks_to_fill=8
example.chemical_tube.ticks_to_fill=16
example.chemical_tank.empty_ticks=64
example.fluid_tank.buckets=32
example.fluid_tank.full=32000
example.bucket.tick1=1000
example.bucket.tick2=0
example.mismatch.accepted=0
example.mismatch.stays=100
example.empty_tube.hydrogen=250
example.empty_tube.oxygen=0
example.break.neighbor=40
example.upgrade.kept=1000
example.sync.threshold=20
example.tube.ultimate=1024000
example.ids=16
```
