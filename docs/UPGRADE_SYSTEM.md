# Upgrade System

Six upgrade items share the eight slots on a processing machine, a factory, and a generator. Speed, energy, and gas already have formulas in [POWER_BUDGET.md](POWER_BUDGET.md). This file adds muffling, filter, and anchor. Caps stay in the power budget: muffling 4, filter 1, anchor 1, speed 8, energy 8, gas 8.

The public muffling stack scales the block's volume down to silence at the cap. The public anchor keeps the block's chunk loaded. Mekanika uses those two effects. The filter upgrade sits in the machine's slots, so it locks that machine's item input.

## Normative constants

```normative
upgrade.muffling.max=4
upgrade.filter.max=1
upgrade.anchor.max=1
upgrade.muffle.volume_den=4
upgrade.anchor.chunks=1
upgrade.slots=8
```

## Items

| Id | Cap | Effect |
| --- | --- | --- |
| `mekanika:upgrade_speed` | 8 | Ticks become `base / (1 + count)`. Joules per tick are multiplied by the same factor |
| `mekanika:upgrade_energy` | 8 | Joules become `amount * 100 / (100 + 10 * count)` |
| `mekanika:upgrade_gas` | 8 | Chemical and fluid millibuckets use that same fraction, at least 1 when the base is at least 1 |
| `mekanika:upgrade_muffling` | 4 | Machine-loop volume |
| `mekanika:upgrade_filter` | 1 | Item-input lock |
| `mekanika:upgrade_anchor` | 1 | One chunk ticket |

Eight speed upgrades take a 200-tick enrichment batch to 22 ticks at 9,000 J/t. Eight energy upgrades take a 1,000 J/t draw to 555 J/t. Factories compute the upgrade once and apply it to every slot.

A stack above the cap is rejected. The extra items stay in the player's cursor. Installed upgrades are not removed to make room.

## Muffling

`mekanika.machine.loop` on that block plays at volume permille `(4 - count) * 1000 / 4`.

| Count | Permille |
| --- | --- |
| 0 | 1000 |
| 1 | 750 |
| 2 | 500 |
| 3 | 250 |
| 4 | 0 |

The radius stays 8 while the permille is above 0. At 0 the loop does not start. A loop that is already playing finishes, and the next restart uses the new permille. UI clicks, scrams, geiger clicks, and transporter sounds are not scaled by this upgrade.

## Filter

Count 0 leaves inserts as they are. Count 1 locks item input:

- If every input slot is empty, the next item that has a recipe is accepted. That id becomes the lock.
- While any input slot holds an item, an insert of a different id accepts 0. An insert of the same id is accepted up to the stack limit.
- Installing the upgrade does not eject items that are already inside.
- A block with no item input, such as a solar generator, keeps the upgrade and does not change its inserts.

An enrichment chamber with the filter and `minecraft:iron_ore` in the input accepts 0 of a 16-count `minecraft:gold_ore` insert. It accepts 16 iron ore when the slot has room. An empty filtered chamber accepts the first ore.

## Anchor

Count 1 force-loads the one chunk that contains the block. The extra radius is 0. The ticket is released when the upgrade is removed or the block is broken. A second anchor item does not fit. Two anchored blocks in the same chunk still load that one chunk.

## Worked-example index

```normative-examples
example.speed8.ticks=22
example.speed8.joules=9000
example.energy8.joules=555
example.muffle.0=1000
example.muffle.1=750
example.muffle.2=500
example.muffle.3=250
example.muffle.4=0
example.muffle.radius=8
example.filter.gold=0
example.filter.iron=16
example.anchor.chunks=1
example.anchor.radius=0
example.caps.muffling=4
example.caps.filter=1
example.caps.anchor=1
```
