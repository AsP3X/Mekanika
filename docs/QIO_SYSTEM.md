# QIO Frequency Storage

Wireless item storage. One frequency is shared by drive arrays, dashboards, importers, and exporters. The public Mekanism QIO model is the pattern: a named frequency, drives with both a count cap and a type cap, and inserts that prefer a drive already holding that item. Capacities below are those public tier numbers. Energy costs are Mekanika's.

## Normative constants

```normative
qio.array.slots=12
qio.drive.basic.count=16000
qio.drive.basic.types=128
qio.drive.advanced.count=128000
qio.drive.advanced.types=256
qio.drive.elite.count=1048000
qio.drive.elite.types=1024
qio.drive.ultimate.count=16000000000
qio.drive.ultimate.types=8192
qio.near_full_permille=750
qio.frequency.name_max=32
qio.filter.max=16
qio.transfer.items_per_tick=8
qio.transfer.joules_per_item=100
qio.transfer.max_items_per_tick=64
qio.chunk_radius=5
```

## Frequency

A frequency id is the pair `(owner UUID, name)`. The name is 1 to 32 characters from `A-Z`, `a-z`, `0-9`, space, `_`, and `-`. Two owners may use the same name. The same owner may not create that name twice. The failure is `message.mekanika.qio.frequency_exists`: "That frequency already exists."

Security is public, private, or trusted, the same modes as [CONTROL_SYSTEM.md](CONTROL_SYSTEM.md). The trusted list is the owner's list from the security desk. A public frequency is visible to everyone. A private one opens only for the owner. A trusted one opens for the owner and the trusted list. Operators bypass when `general.operator_bypass` is true, and creative players bypass when `general.creative_bypass` is true. Both default to false. The rule is in [CONTROL_SYSTEM.md](CONTROL_SYSTEM.md).

Each frequency stores a dye color 0 through 15. It is shown on the dashboard and does not affect storage.

The frequency lives in world data, not in a chunk. Dashboards in loaded chunks can read and write it while the drive array's chunk is unloaded. The cache is the working copy. On array unload, and on drive removal, each drive item is rewritten with its count and type list. If the world saves in between, the drive items and the frequency cache match.

## Drives

| Drive | Items | Types |
| --- | --- | --- |
| `mekanika:qio_drive_basic` | 16,000 | 128 |
| `mekanika:qio_drive_advanced` | 128,000 | 256 |
| `mekanika:qio_drive_elite` | 1,048,000 | 1,024 |
| `mekanika:qio_drive_ultimate` | 16,000,000,000 | 8,192 |

A type slot is one item identity: the item id plus its components. Two damaged tools of the same id are two types. A stack's count can pass 64 inside the frequency. Pulling into an inventory still respects that item's max stack.

The drive array is one block, `mekanika:qio_drive_array`, with 12 slots in two rows of six. It is not a multiblock and has no proxy blocks. Status per slot:

| Status | When |
| --- | --- |
| `none` | Empty slot |
| `offline` | A drive is present and the array has no frequency |
| `ready` | Stored count is below 75% and type count is below the type cap |
| `near_full` | Count is at least 75% of the count cap, or type count equals the type cap |
| `full` | Count equals the count cap |

75% of a basic drive is 12,000 items.

## Insert and extract

Drives are ordered by array block position `(y, z, x)`, then slot 0 through 11.

To insert a type:

1. Walk drives that already store that type and fill their remaining count.
2. If some items remain, walk drives that still have a free type slot and free count.
3. Stop when the items are stored or every drive refuses them. The remainder stays in the source.

An insert that would need a new type while every drive is at its type cap stores nothing of that new type. Count and types are checked together: a drive with free count and no free type slot accepts only types it already has.

Extract removes from those same drives in that order. The viewer shows the summed count.

Twelve basic drives hold 192,000 items and 1,536 types. One ultimate drive holds 16,000,000,000 items and 8,192 types.

## Dashboard

`mekanika:qio_dashboard` opens the viewer. `mekanika:portable_qio_dashboard` opens the same viewer from the inventory. Both store a frequency id and a color.

Search is a case-insensitive substring of the display name. A query that starts with `#` matches a tag instead. Empty search lists every type, sorted by display name.

Click takes one item. Shift-click takes up to the item's max stack. Shift-clicking a stack from the player inventory inserts as many as the frequency accepts.

Every click checks security again. If the check fails, the screen closes and the player gets `message.mekanika.qio.denied`: "You cannot use this frequency."

The dashboard comparator reads `floor(totalCount * 15 / totalCountCapacity)`, or 0 when the capacity is 0. Layout is [QIO_GUI.md](QIO_GUI.md).

## Importer and exporter

`mekanika:qio_importer` pulls from the inventory on its back face into the frequency. `mekanika:qio_exporter` pushes from the frequency into that inventory. They tick only while their chunk and the neighbor chunk are loaded. A move is simulated first. If the neighbor is unloaded before the execute, the move is skipped.

Default rate is 8 items per tick, at 100 J each, so 800 J/t. Config `qio.transfer.items_per_tick` clamps from 1 to 64. The block's energy buffer holds 20 ticks of that draw.

Each has up to 16 filters. A filter is an item id, a tag, or a mod id. An importer with no filters accepts every item the neighbor offers. An exporter with no filters moves nothing.

Filters and the frequency are copied by a config card. The card does not copy stored items. Pasting a frequency succeeds only when the pasting player can open that frequency. The block's owner does not change.

Redstone uses the modes in the control doc. While redstone stops the block, it does not drop its frequency.

## Crafts

Ultimate-tier parts, from [MATERIAL_PROGRESSION.md](MATERIAL_PROGRESSION.md):

| Item | Craft |
| --- | --- |
| Basic drive | 4 infused alloy + 1 advanced circuit |
| Advanced drive | 4 reinforced alloy + 1 elite circuit |
| Elite drive | 4 atomic alloy + 1 ultimate circuit |
| Ultimate drive | 4 elite drives + 4 atomic alloy |
| Drive array | 4 atomic alloy + 2 ultimate circuits |
| Dashboard | 2 atomic alloy + 1 ultimate circuit + 1 glass |
| Portable dashboard | 1 dashboard + 1 energy tablet |
| Importer or exporter | 2 atomic alloy + 1 ultimate circuit |

## Worked-example index

```normative-examples
example.basic.near_full_count=12000
example.array.basic.count=192000
example.array.basic.types=1536
example.ultimate.count=16000000000
example.ultimate.types=8192
example.insert.new_type_on_full_basic=0
example.transfer.default_joules_per_tick=800
example.exporter.empty_filter_items=0
```
