# Failure Modes

Player-visible results. The English lines already fixed in other docs are repeated here so a test can compare one catalog. New failures use a new key. They do not reuse `invalid_shape`.

## Normative constants

```normative
failure.chat_radius_blocks=8
failure.formation_particle=mekanika.particle.fail
failure.access_denied=Access denied.
failure.qio_denied=You cannot use this frequency.
```

Chat for a formation failure goes to players within 8 blocks and to the security owner. The same code and position are not sent again until the structure forms or the code changes.

## Multiblock

The code, the lang key, and the English are the failure table in [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md). Each of those also plays `mekanika.multiblock.fail` and spawns `mekanika.particle.fail` at the position. `chunk_unloaded` does not chat.

A boiler scram sends "Boiler emergency vent at %s". A fission scram sends "Fission reactor scrammed at %s". A fusion quench sends "Fusion reaction quenched at %s". The structure stays formed for a scram. A quench returns fusion to idle.

Sneak is required to break a controller that still holds energy or a tank amount. The line is "Sneak to break this controller and void its contents".

## Radiation

Scale names and damage are in [RADIATION_SYSTEM.md](RADIATION_SYSTEM.md). The first cross of severity 0.1 sends "Radiation sickness." A later sample that falls to Safe sends "Radiation is falling." Geiger sounds follow the scale. Safe is silent. Walls do not stop the field. A lined suit or a radiation module is what reduces the dose.

## Machines and networks

| Condition | What the player sees |
| --- | --- |
| Redstone stops the machine | Status `redstone_off` in the GUI. No chat |
| Missing input or fuel | The machine's status token: `no_fuel`, `needs_water`, `missing_input`, or `output_full` |
| Security fails on a block | `Access denied.` |
| Security fails on a QIO frequency | `You cannot use this frequency.` The open screen closes |
| Item path cannot be finished | The stack waits, then drops after 100 idle ticks. `mekanika.transporter.reject` plays when a pull fails |
| Heat deleted by a tier downgrade | "Stored heat exceeded the new capacity and the extra was removed" |
| Debug tool denied | "Debug tools are disabled." |
| Debug look misses | "No Mekanika block in sight." |

A full energy or chemical buffer refuses the extra. It does not delete the source stack. Item transporters drop only after the idle timeout in the transport doc.

## Gear

Flight stops on the tick the chest cannot pay. Fall damage is cancelled only when `fall_unit` can pay 40 J. A module install that conflicts does not consume the module item. The GUI shows the conflict.

## Worked-example index

```normative-examples
example.chat_radius=8
example.qio_denied=You cannot use this frequency.
example.access_denied=Access denied.
example.safe_geiger=0
example.idle_drop_ticks=100
```
