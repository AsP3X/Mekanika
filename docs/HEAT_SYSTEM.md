# Heat System

Conductor heat is an integer point value. It is not Joules. Boilers are the machine that turns it into steam. The boiler queue, boil thresholds, and steam temperature rules stay in [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md). This document owns conductors, dissipation, and the link math.

Fission heat stays inside the fission reactor. Fusion does not use conductor heat. A boiler does not pull heat out of either reactor. Fission moves energy by sending steam to a turbine.

## Normative constants

```normative
heat.ambient_deci_c=200
heat.display_span_deci_c=4800
heat.leak_divisor=60000
heat.rebuild_period_ticks=10

conductor.basic.capacity=1000
conductor.basic.rate=40
conductor.basic.leak_permille=20
conductor.advanced.capacity=4000
conductor.advanced.rate=120
conductor.advanced.leak_permille=10
conductor.elite.capacity=16000
conductor.elite.rate=360
conductor.elite.leak_permille=4
conductor.ultimate.capacity=64000
conductor.ultimate.rate=1000
conductor.ultimate.leak_permille=1
```

Display temperature uses the same scale as a boiler:

`displayDeciC = 200 + heatBuffer * 4800 / capacity`

An empty buffer reads 20.0°C. A full buffer reads 500.0°C. Capacity is never zero on a formed conductor or a heat generator.

## Blocks

| Id | Role |
| --- | --- |
| `mekanika:thermodynamic_conductor_basic` | Conduction and a small buffer |
| `mekanika:thermodynamic_conductor_advanced` | Higher rate, lower leak |
| `mekanika:thermodynamic_conductor_elite` | Higher rate, lower leak |
| `mekanika:thermodynamic_conductor_ultimate` | Highest rate, lowest leak |

Conductors are block entities. They store the heat buffer, the leak accumulator, and six face modes. They do not store a network id. The network is rebuilt.

Tier installers from [TIER_UPGRADING.md](TIER_UPGRADING.md) change the conductor tier and keep the heat amount, clamped to the new capacity. Extra heat above the new capacity is deleted. The owner gets `message.mekanika.heat.heat_voided`.

## Faces

Each face is `connect` or `none`. The default is `connect`. A wrench or the Configurator cycles the face. `none` does not connect and counts as an open face for leaks. Two conductors couple only when both touching faces are `connect`.

A `connect` face also couples to:

- A heat-generator face. Generators always accept that coupling.
- A boiler casing. Valves, glass, and the boiler controller do not accept heat. That rule is in the structure spec.

Conductors do not couple to fission casings, turbines, fusion frames, or energy cables.

## Networks

A network is the six-connected set of conductors that can reach each other through `connect` faces. Heat generators and boiler casings are attachments. They are not conductor nodes.

Placing or breaking a conductor, or changing a face mode, marks the network dirty. The rebuild runs on the next 10-tick period. Until then, links whose blocks are gone are skipped.

Each link is an unordered pair. Sort links by the lower block position, then the higher one, as `(y, z, x)` tuples. Process that order once per tick. Earlier links see the buffer values later links will use.

The link rate is the minimum of the two endpoints' rates. A boiler attachment uses the conductor or generator rate on the other side. A heat generator's rate is its current heat output per tick, from [GENERATORS_REACTORS.md](GENERATORS_REACTORS.md).

## Transfer

For a link between A and B:

`raw = heatA * capacityB - heatB * capacityA`

When `raw` is positive, A is hotter and `q = raw / (capacityA + capacityB)`. When `raw` is negative, B is hotter and `q = (-raw) / (capacityA + capacityB)`. Integer division. `raw` of 0 moves nothing.

`proposed = min(q, linkRate, sourceBuffer)`

When the sink is a conductor or a heat generator, `accepted = min(proposed, sinkCapacity - sinkBuffer)`. Subtract `accepted` from the source and add it to the sink.

When the sink is a boiler casing, `accepted = acceptHeat(proposed)` on that boiler. The source loses only `accepted`. `acceptHeat` queues heat. It does not boil inside the conductor tick. The boiler tick in the structure spec commits the queue.

Equal capacities of 1000, buffers 800 and 200, and a basic rate of 40:

`q = (800 * 1000 - 200 * 1000) / 2000 = 300`

`proposed = 40`. After the tick the buffers are 760 and 240.

A basic conductor at 800 points beside an empty boiler element (`capacity` 1000, buffer 0):

`q = (800 * 1000 - 0 * 1000) / 2000 = 400`

The link offers 40. That is the offer used by the 3×3×3 boiler trace in the structure spec, once this conductor is the source.

## Dissipation

Open faces are faces that are not coupled to a heat node this tick. A face set to `none` is open. A face against stone is open. A coupled face is closed.

Each conductor tick, before links:

`leakAccum += heatBuffer * leakPermille * openFaces`

`loss = leakAccum / 60000`

`leakAccum = leakAccum % 60000`

Subtract `loss` from the buffer, floored at 0.

A full basic conductor (buffer 1000, leak 20, 6 open faces) adds 120,000 to a zero accumulator and loses 2 points. The remainder is 0.

Ultimate conductors leak 1 permille, so a buried ultimate line keeps its heat until a boiler takes it. Basic lines need a short path.

## Tick order

1. Heat generators commit this tick's produced heat into their buffers, capped at capacity. Extra produced heat is discarded.
2. Conductors dissipate.
3. Process links in the sorted order above.

Energy production of a generator is not part of this tick. The generator document owns the Joule buffer.

## Save and sync

Save `mekanika:data_version` (1), `heat_buffer`, `leak_accum`, and six face modes. Rebuild the network on load.

Sync the displayed temperature and the last accepted flow at most every 10 ticks. A debug inspection syncs immediately.

A saved data version newer than 1 clears the buffer and logs an error.

## Player feedback

| Condition | Key | English |
| --- | --- | --- |
| Heat deleted by a tier downgrade | `message.mekanika.heat.heat_voided` | Stored heat exceeded the new capacity and the extra was removed |
| Boiler refused the offer because its queue is full | GUI status on the conductor | Sink full |

The boiler's own scram text stays in the structure spec. Conductors do not scram and do not break when full. They refuse incoming heat once their buffer is full.

Sound event `mekanika.heat.vent` may play while a conductor's display is at least 1000 and it lost heat to an open face. Particle choice stays in [SOUNDS_AND_PARTICLES.md](SOUNDS_AND_PARTICLES.md).

## Debug fields

Extend the network reader with a heat section:

- Display temperature
- Buffer and capacity
- Last link flow in and out
- Open faces
- Leak accumulator
- Network conductor count
- Attached boiler and generator positions

## Worked-example index

```normative-examples
example.conductor.q_equal=300
example.conductor.equal_move=40
example.conductor.after_hot=760
example.conductor.after_cold=240
example.conductor.boiler_offer=40
example.conductor.leak_basic_full_open=2
```
