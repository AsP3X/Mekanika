# Radiation System

How reactor strengths become player dose. Fission and fusion publish sieverts per hour in [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md). This document owns falloff, decay, shielding, and effects.

The falloff, the scale cuts, and the decay rates match Mekanism's public radiation manager. The suit values and the waste-barrel rules below are Mekanika's.

## Normative constants

```normative
radiation.enabled=true
radiation.baseline_sv_per_hour=0.0000001
radiation.min_source_sv_per_hour=0.00001
radiation.source_decay=0.9995
radiation.body_decay=0.9995
radiation.chunk_radius=5
radiation.sample_chance_denominator=20
radiation.sv_per_hour_to_sv=3600
radiation.scale_low=0.00001
radiation.scale_moderate=0.001
radiation.scale_high=0.1
radiation.scale_critical=10
radiation.effect_severity=0.1
radiation.severity_log_min=-5
radiation.severity_span=7
radiation.damage_severity_major=0.5
radiation.damage_severity_critical=0.8
radiation.barrel_capacity_mb=512000
radiation.barrel_decay_mb=1
radiation.barrel_period_ticks=20
radiation.suit_piece_resistance=0.2
radiation.resistance_cap=1
```

`radiation.severity_span` is `log10(100) - log10(0.00001) = 7`.

## Sources

Two kinds of source share a block position.

A **held source** is replaced every tick by the machine that owns it. A burning fission reactor sets its controller to `assemblies * (100 - insertion) + wasteMb / 1000` sieverts per hour. A running fusion reactor sets its controller to 2. When the machine stops publishing, the held source is removed. It does not decay while the machine is publishing.

A **burst source** is added once and then decays. Fission scram adds `assemblies * 1000`. Fusion break adds 500. Dumping spent fuel adds `millibuckets / 1000`. These adds stack on the burst at that position. They are not cleared by the held source.

A source chunk stores both. Only chunks within `radiation.chunk_radius` (5) of the player are summed. That is 80 blocks.

## Level at a position

`level = baseline + sum over sources of magnitude / max(1, distSqr)`

`distSqr` is the squared Euclidean distance between block positions. The same block uses 1, so the player standing on the source receives the full magnitude.

Walls do not reduce this. Armor and modules do, and only when the dose is applied to a body.

| Situation | Level | Scale |
| --- | --- | --- |
| One fission assembly, insertion 0, on the controller | 100 Sv/h | Critical |
| Same assembly, 10 blocks away (`distSqr` 100) | 1 Sv/h | High |
| Fusion running, on the controller | 2 Sv/h | High |
| Scram burst 1000, 10 blocks away, before decay | 10 Sv/h | Critical |

Scale names match the HUD words:

| Environmental level | Name | HUD color |
| --- | --- | --- |
| Below 0.00001 Sv/h | Safe | Green |
| Below 0.001 Sv/h | Low | Yellow |
| Below 0.1 Sv/h | Moderate | Orange |
| Below 10 Sv/h | High | Red |
| 10 Sv/h and above | Critical | Dark red |

## Dose

About once a second, each living entity rolls `1 / 20`. On a hit, if `level` is above baseline and the entity is not a creative player, add:

`dose += (level / 3600) * (1 - min(1, resistance))`

A full second at 1 Sv/h with no suit adds `1/3600` Sv, about 0.0002778 Sv.

The same roll multiplies an existing body dose by `0.9995`, and multiplies every burst source by `0.9995`. A burst below `0.00001` Sv/h is removed. A scram burst of 1000 Sv/h is 999.5 after one decay.

Body dose uses the same decay. Creative players store no dose and do not roll.

## Severity and harm

`severity = clamp((log10(dose) - (-5)) / 7, 0, 1)` when dose is at least `0.00001` Sv. Otherwise severity is 0.

Harm starts at severity 0.1, which is about `0.00005012` Sv. Each successful sample that is still at or above that severity applies damage:

| Severity | Damage |
| --- | --- |
| 0.1 up to 0.5 | 1 |
| 0.5 up to 0.8 | 4 |
| 0.8 and above | 10 |

At a steady 1 Sv/h and no suit, severity 0.1 is `10^-4.3` Sv, about 0.00005012 Sv. Reaching it takes 0.1804 seconds, so the next once-per-second roll after standing in that field applies damage. At 0.0005 Sv/h the same threshold takes 360.85 seconds.

The hurt sound and the chat line `message.mekanika.radiation.sick` fire when severity first crosses 0.1. `message.mekanika.radiation.recovering` fires when a later sample sees the dose falling and the scale drop to Safe. Text: "Radiation sickness." and "Radiation is falling."

## Shielding

Resistance adds and then caps at 1. Each piece of `mekanika:lined_helmet`, `lined_chest`, `lined_legs`, and `lined_boots` adds 0.2. Four pieces are 0.8. A later gear module may add the rest. At 1.0 the dose added that sample is 0.

The dosimeter `mekanika:dosimeter` adds no resistance. Using it prints the current Sv/h and the scale name to that player.

`mekanika:spent_fuel` in an inventory is a held source on that entity of `itemCount` Sv/h, because one item stands for 1,000 mB and the waste rule divides by 1,000. The barrel below is the safe container.

## Waste barrel

`mekanika:waste_barrel` stores up to 512,000 mB of `mekanika:spent_fuel`. While the chemical is inside, it is not a source. Every 20 ticks the barrel destroys 1 mB. Breaking the barrel or dumping the tank adds `millibuckets / 1000` to the burst source at that position.

## HUD

[RADIATION_HUD.md](RADIATION_HUD.md) reads `level` for the scale and color, and body dose for the bar. The bar fill is the severity above. Geiger sounds are `mekanika.geiger.slow` at Low, `mekanika.geiger.medium` at Moderate, and `mekanika.geiger.fast` at High and Critical. Safe is silent.

## Debug

The network reader prints, for the targeted block and the player:

- Environmental Sv/h and scale name
- Body dose and severity
- Held magnitude, burst magnitude, and source position
- Resistance total

## Worked-example index

```normative-examples
example.fission.contact.sv_per_hour=100
example.fission.contact.scale=critical
example.fission.range10.sv_per_hour=1
example.fission.range10.scale=high
example.fusion.contact.sv_per_hour=2
example.fusion.contact.scale=high
example.scram.range10.sv_per_hour=10
example.scram.range10.scale=critical
example.dose.one_sv_per_hour=0.0002777777777777778
example.decay.burst_1000=999.5
example.harm.seconds_at_1_sv=0.1804
example.harm.seconds_at_0_0005_sv=360.85
example.suit.four_pieces=0.8
```
