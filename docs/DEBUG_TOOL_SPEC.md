# Debug Tool

One item and one command print the same report. They do not change the block unless the report says a field is writable, and none of these fields are writable.

## Normative constants

```normative
debug.command=/mekanika debug
debug.item=mekanika:network_reader
debug.requires=debug_mode_or_creative
debug.chat_lines=20
debug.full_sync=true
```

`general.debug_mode` is the config flag. Creative players can use the tool when the flag is false. Survival players cannot. A denied use sends `message.mekanika.debug.denied`: "Debug tools are disabled."

## Use

`/mekanika debug` prints the report for the block the player is looking at, within 8 blocks. The command fails with `message.mekanika.debug.no_target` when the look ray misses: "No Mekanika block in sight."

The network reader prints the same report on use. Sneak-use prints the last 20 lines again. Either use asks the server for `mekanika:block_full` before printing, so the numbers match the server.

## Report

The header is the block id and position. Then only the sections that apply:

| Section | Lines |
| --- | --- |
| Energy | Stored, capacity, last insert, last extract |
| Chemical or fluid | Id, amount, capacity, temperature for steam |
| Items | In-transit count, color, progress, remaining path length |
| Sides | Six faces and the mode for each transmission the block supports |
| Control | Redstone mode, security mode, owner name |
| Multiblock | Formed flag, size, last failure code and position |
| Heat | Buffer, capacity, display temperature, open faces |
| Radiation | Sv/h and scale name at the block, body dose of the player |
| QIO | Frequency name, total count, count cap, type count, type cap |
| Gear | Energy, module id, level, enabled flag |

System docs add the extra fields for that block. Those fields append to the matching section. A boiler appends steam rate and the scram flag to Heat. A transporter appends delay and filter count to Items.

## Worked-example index

```normative-examples
example.command=/mekanika debug
example.range_blocks=8
example.chat_lines=20
example.survival_without_flag=denied
example.creative_without_flag=allowed
```
