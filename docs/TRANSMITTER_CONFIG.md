# Transmitter Configuration

Shared face rules for cables, pipes, tubes, transporters, and conductors. Item pathfinding, colors, and sorter filters are specified in [ITEM_TRANSPORT.md](ITEM_TRANSPORT.md). Conductor rates are in [HEAT_SYSTEM.md](HEAT_SYSTEM.md). Cable, tube, pipe, and tank numbers, and the per-tick split, are in [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md).

## Normative constants

```normative
transmitter.colors=16
transmitter.default_mode=normal
transmitter.buffered_link=min
transmitter.item_cost=per_block
```

## Faces

Each face is `normal`, `push`, `pull`, or `none`. The default is `normal`.

| Mode | Connects | Pulls | Sends into an inventory |
| --- | --- | --- | --- |
| `normal` | Yes | No | Yes |
| `push` | Yes | No | Yes |
| `pull` | Yes | Yes | Yes |
| `none` | No | No | No |

The Configurator cycles the mode on the face you click. Sneak-use cycles color on an item transporter. The wrench mode still rotates or removes, as in [CONTROL_SYSTEM.md](CONTROL_SYSTEM.md).

A config card copies face modes, color, and filters. It does not copy items that are in transit.

## Color

Item transporters have sixteen dye colors plus none. The connection rule and the stack color rule are in the item transport doc. Cables, pipes, tubes, and conductors have no color.

## Tiers

Buffered links (energy, chemical, fluid, heat) move at the slower of the two endpoints. A basic cable against an ultimate cable has a link rate of 1,000 J/t. A full basic buffer beside an empty ultimate buffer moves 992 J on that tick and 0 on the next, from [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md). Heat uses the same minimum and its own leak rule.

Item transporters do not slow the whole network to the worst block. Each block adds its own path cost and advances a stack at its own speed. A one-block ultimate path costs 1. A one-block basic path costs 10. The cheaper path is chosen.

## What is stored

Each transmitter saves its six face modes. An item transporter also saves its color and any filters. In-transit stacks are saved by the item transport doc. A missing key loads as `normal` and color none.

## Debug

The reader shows the six modes, the color when the block has one, the tier, and the link rate. For a buffered link the rate is the minimum of the two ends. For an item block the rate shown is that block's speed.
