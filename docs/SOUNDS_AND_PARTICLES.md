# Sounds and Particles

Event ids, what starts them, and when they stop. Assets are original. The client keys `client.sounds.volume` and `client.particles.enabled` are in [CONFIG_STRUCTURE.md](CONFIG_STRUCTURE.md).

## Normative constants

```normative
sound.machine_loop.seconds=2
sound.machine_loop.radius_blocks=8
sound.geiger.levels=3
particle.formation.ticks=20
particle.vent.min_deci_c=1000
sound.volume.default=1
```

A looping sound restarts when the block is still active at the end of the loop. It stops on the tick the block goes idle. Sounds at the same block do not stack. A second trigger replaces the first. `mekanika.machine.loop` uses the muffling permille in [UPGRADE_SYSTEM.md](UPGRADE_SYSTEM.md). At permille 0 that loop does not start. Other events keep `sound.volume.default`.

## Sounds

| Event | Trigger | Stop |
| --- | --- | --- |
| `mekanika.machine.loop` | A processing machine, factory, or generator is running | The block leaves `running` |
| `mekanika.multiblock.form` | A structure forms | One shot |
| `mekanika.multiblock.fail` | A formation failure chat line is sent | One shot, at the failure position |
| `mekanika.ui.click` | A side mode, redstone mode, or module enable changes | One shot, on that client |
| `mekanika.boiler.scram` | Boiler scram | One shot |
| `mekanika.fission.scram` | Fission scram | One shot |
| `mekanika.fusion.ignite` | Fusion enters `running` | One shot |
| `mekanika.fusion.quench` | Fusion quenches | One shot |
| `mekanika.heat.vent` | A conductor at display 1000 or above loses heat to an open face | The condition ends |
| `mekanika.geiger.slow` | Radiation scale Low | The scale changes |
| `mekanika.geiger.medium` | Radiation scale Moderate | The scale changes |
| `mekanika.geiger.fast` | Radiation scale High or Critical | The scale changes |
| `mekanika.transporter.insert` | A transporter accepts a stack | One shot |
| `mekanika.transporter.reject` | A pull fails because the destination and the source both refuse | One shot |

Safe radiation is silent. Turbine hum is `mekanika.machine.loop` on the controller while status is not `offline`.

## Particles

| Event | Trigger | Count |
| --- | --- | --- |
| `mekanika.particle.form` | Structure forms | One burst, 20 ticks, on the frame |
| `mekanika.particle.fail` | Formation failure | One burst at the failure position |
| `mekanika.particle.work` | Machine loop is playing | One particle every 10 ticks at the front face |
| `mekanika.particle.steam` | A turbine or boiler is producing steam | One particle every 10 ticks at a steam valve |
| `mekanika.particle.heat` | `mekanika.heat.vent` is playing | One particle every 10 ticks |

Particles are client-only. The server does not send a particle packet when `client.particles.enabled` is false on that client. Formation and failure bursts still play for players inside 32 blocks, because they mark a position the chat line also names.

## Worked-example index

```normative-examples
example.sounds.one_shot=9
example.sounds.loops=5
example.geiger.levels=3
example.particle.work.period_ticks=10
example.safe_radiation.sounds=0
```
