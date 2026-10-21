# NBT and Data Components

What is saved, and the key names. Each system doc lists the fields that belong to that block. This file is the shared shape. Version is 1. A saved version newer than 1 keeps the block, clears Mekanika fields, and logs an error. It does not guess a migration.

## Normative constants

```normative
nbt.version=1
nbt.version_key=mekanika:data_version
nbt.bucket_mb=1000
nbt.omit_empty=true
nbt.omit_defaults=true
```

Empty stacks and default face modes are omitted. A missing key loads as the default from the system doc.

## Items

Items use data components.

| Component | Payload |
| --- | --- |
| `mekanika:data_version` | int |
| `mekanika:energy` | long joules |
| `mekanika:j_to_fe_carry` | int, Forge and NeoForge bridge, 0 through `joule_denominator - 1` |
| `mekanika:fe_to_j_carry` | int, Forge and NeoForge bridge, 0 through `fe_numerator - 1` |
| `mekanika:modules` | list of `{id: string, level: int, enabled: bool}` |
| `mekanika:mode` | string, disassembler and configurator |
| `mekanika:hydrogen` | long millibuckets |
| `mekanika:oxygen` | long millibuckets |
| `mekanika:fluids` | fluid tank item, one `{id, amount}` |
| `mekanika:chemicals` | chemical tank item, list of `{id, amount, temperature_deci_c}` |
| `mekanika:frequency` | `{owner: uuid, name: string}` |
| `mekanika:color` | int 0 through 15 |
| `mekanika:drive` | `{count: long, entries: list of {item: string, count: long}}` |

A QIO drive's `entries` length is at most that drive's type cap. The sum of `count` is at most the count cap. A drive that breaks either rule loads as empty and logs the item id.

## Block entities

Block entities save a compound. Keys use the same `mekanika:` prefix.

| Key | Payload |
| --- | --- |
| `mekanika:data_version` | int |
| `mekanika:energy` | long |
| `mekanika:j_to_fe_carry` | int, same range as the item component |
| `mekanika:fe_to_j_carry` | int, same range as the item component |
| `mekanika:progress` | int ticks into the current batch. A factory keeps this at 0 |
| `mekanika:slot_progress` | list of ints, one per factory slot |
| `mekanika:auto_sort` | bool, factory only. Missing means off |
| `mekanika:fluids` | list of `{id, amount}` |
| `mekanika:chemicals` | list of `{id, amount, temperature_deci_c}` |
| `mekanika:items` | list of item stacks in slot order |
| `mekanika:sides` | six faces, each a mode name per transmission type the block supports |
| `mekanika:redstone` | `ignored`, `high`, `low`, or `pulse`. Missing means `ignored` |
| `mekanika:redstone_level` | int 0 through 15, the previous signal |
| `mekanika:pulse_armed` | bool |
| `mekanika:logic_mode` | string, logic adapters. Missing means `disabled` |
| `mekanika:security` | `{owner: uuid, mode: string}` |
| `mekanika:color` | transporter color, or omitted for none |
| `mekanika:filters` | list of `{kind: item\|tag\|mod, value: string}` |
| `mekanika:transit` | list of in-flight item stacks with path, progress, and color |
| `mekanika:heat_buffer` | int, conductors and boilers |
| `mekanika:leak_accum` | int, conductors |

A cable uses `mekanika:energy` for its buffer. A tube uses one `mekanika:chemicals` entry. A pipe uses one `mekanika:fluids` entry. The amounts and the rule that a broken pipe deletes only its own buffer are in [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md).

Multiblock controllers save contents and settings. They do not save the block position list. That list is rebuilt on load. The structure chapters name the extra fields (scram, insertion, recipe progress).

Frequencies, including QIO, are world data under `mekanika:frequencies`. They are not copied onto every dashboard. A dashboard saves only `mekanika:frequency`.

## What is not saved

Network membership, pathfinder caches, acceptor lists, and derived rates are rebuilt. A client never writes these compounds. The server is the only writer.

## Worked-example index

```normative-examples
example.version=1
example.bucket_mb=1000
example.omit_empty_stack=true
example.drive.over_type_cap=empty
example.frequency.on_dashboard=id_only
```
