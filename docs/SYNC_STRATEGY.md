# Client/Server Sync

What the server sends, and when. The client does not invent amounts. Networks are rebuilt on the server from blocks. They are not stored in a packet.

## Normative constants

```normative
sync.period_ticks=10
sync.delta_divisor=100
sync.progress_scale=100
sync.gui_opens_full=true
sync.formation_immediate=true
sync.sides_immediate=true
```

## Packets

| Id | When | Payload |
| --- | --- | --- |
| `mekanika:block_delta` | See the delta rule | Block position, energy, fluid amounts, chemical amounts, progress, status |
| `mekanika:block_full` | A player opens a GUI, or the debug reader requests it | Every saved Mekanika field for that block |
| `mekanika:formation` | A structure forms or unforms | Formed flag, bounds, last failure code, position |
| `mekanika:sides` | A face mode changes | Position and the six modes that changed |
| `mekanika:radiation` | The player's scale name changes | Sv/h and the scale name |

A delta is due when 10 ticks have passed since the last send, or when the absolute change in a long amount is at least `max(1, capacity / 100)`. Progress uses the same 10-tick period. Side modes and formation do not wait.

A basic cable buffer of 1,000 J has a threshold of 10 J. A change of 9 waits for the period. A change of 10 sends on that tick.

Opening a GUI sends `block_full` to that player only. Other players keep the delta stream. Two players with the GUI open each have their own full snapshot. Closing the GUI returns that player to the delta stream.

## Client

The client stores the last progress and the last server tick. Between packets it advances a displayed progress by the block's speed, and it never draws past `sync.progress_scale` (100) until the server says the batch finished. A correction packet replaces the displayed value. It does not animate backward across more than one batch.

Item stacks in transporters interpolate from the progress field. The path itself is not predicted. If the next packet removes the stack, the client removes it.

Unloaded chunks drop client copies. The next load sends a full snapshot with the chunk.

## Radiation and QIO

Radiation uses `mekanika:radiation` only when the scale name changes, so a steady field does not send a packet every sample. Body dose for an open dosimeter uses `block_full` rules while that item screen is open.

A QIO viewer sends type deltas: item id and new total count. A count of 0 removes the row. The viewer does not receive other players' inventories.

## Worked-example index

```normative-examples
example.period_ticks=10
example.cable_1000.threshold=10
example.cable_1000.change_9_waits=true
example.cable_1000.change_10_sends=true
example.cube_1000000.threshold=10000
example.progress_cap=100
```
