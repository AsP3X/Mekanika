# Performance Guidelines

Budgets for the tick loop. The formation period, the sync period, and the transporter search cap are already normative elsewhere. This file sets the global caps those systems share.

## Normative constants

```normative
perf.formation_period_ticks=10
perf.max_exterior=18
perf.max_validations_per_tick=4
perf.max_rebuilds_per_tick=8
perf.search_node_cap=1024
perf.sync_period_ticks=10
perf.particle_period_ticks=10
perf.qio_deltas_per_packet=64
perf.reference_cuboid_blocks=5832
```

`18 × 18 × 18` is 5,832 blocks. One validation may scan that many block states and then stop. A second structure waits. At most 4 validations start in one tick. The rest stay dirty until a later period.

At most 8 transmitter or conductor rebuilds start in one tick. Item path searches visit at most 1,024 transporters, the cap in [ITEM_TRANSPORT.md](ITEM_TRANSPORT.md).

A QIO viewer packet contains at most 64 type rows. Further rows wait for the next 10-tick sync.

## Tick path

Hot loops do not allocate a new list, map, or stack wrapper per block. Acceptor lists and path results are cached until the network is dirty. Interior multiblock checks read block states. They look up a block entity only for the controller and for ports.

Energy and chemical amounts use `long`. A multiply that can exceed `Long.MAX_VALUE` is rejected and the last safe value is kept.

## What to measure

Before calling step 11 done, time one validation of an 18×18×18 hollow tank and one insert of 64 items into a 12-drive QIO array. Record the milliseconds. The cap is the validation and rebuild counts above, not a wall-clock number, because the machine under test is not fixed.

## Worked-example index

```normative-examples
example.cuboid_blocks=5832
example.validations_per_tick=4
example.rebuilds_per_tick=8
example.search_node_cap=1024
example.qio_deltas=64
```
