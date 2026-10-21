# GUI Layouts

Pixel positions for every screen. Claude paints the two texture files. Grok places widgets at these coordinates. Interaction rules that already name a click, a search, or a refusal stay in the screen's own doc.

The public mod draws its frame as a 9-slice and uses an 18-pixel slot. Mekanika uses that structure. The paintings are original.

## Normative constants

```normative
gui.atlas=256
gui.frame=9
gui.frame.corner=4
gui.slot=18
gui.player.columns=9
gui.player.rows=3
gui.player.inv_from_bottom=82
gui.player.hotbar_from_bottom=24
gui.upgrade.count=8
gui.upgrade.span=144
gui.upgrade.from_bottom=150
gui.machine.width=200
gui.machine.height=176
gui.factory.row=22
gui.factory.height_base=110
gui.small.width=176
gui.small.height=166
gui.qio.columns=8
gui.qio.rows=6
gui.qio.width=252
gui.qio.height=222
gui.side.width=176
gui.side.height=110
gui.gear.columns=4
gui.gear.rows=8
gui.gear.height=258
gui.security.rows=5
gui.security.height=228
gui.screens=17
```

`110 + slots * 22` is the factory image height. A basic factory is 176. An ultimate factory is 308. The player inventory's first row is `imageHeight - 82`. The hotbar is `imageHeight - 24`. The upgrade column's first slot is `imageHeight - 150`. Eight slots span `8 * 18 = 144`.

## Textures

| File | Pixels | Use |
| --- | --- | --- |
| `assets/mekanika/textures/gui/frame.png` | 9×9 | Screen frame. The corner is 4. The center pixel stretches. |
| `assets/mekanika/textures/gui/widgets.png` | 256×256 | Slots, gauges, arrows, and buttons |

Widget rectangles inside `widgets.png`:

| Name | X | Y | W | H |
| --- | --- | --- | --- | --- |
| Slot | 0 | 0 | 18 | 18 |
| Energy gauge | 18 | 0 | 16 | 52 |
| Progress empty | 0 | 18 | 24 | 16 |
| Progress full | 24 | 18 | 24 | 16 |
| Button | 0 | 34 | 16 | 16 |
| Gauge frame | 16 | 34 | 16 | 52 |

A slot hit box is the 18×18 rectangle. The item icon is the inner 16×16. Coordinates below are the top-left of the hit box, relative to the screen's top-left. The screen is centered on the Minecraft window.

## Player inventory

Every screen except the side screen draws it.

| Row | X | Y |
| --- | --- | --- |
| Main, three rows | `8 + column * 18` | `imageHeight - 82 + row * 18` |
| Hotbar | `8 + column * 18` | `imageHeight - 24` |

Column and row start at 0. Nine columns. A 176×166 screen puts the main inventory at y=84 and the hotbar at y=142.

## Colors

Face buttons use these RGB colors. The names match [SIDE_CONFIGURATION.md](SIDE_CONFIGURATION.md).

| Mode | R | G | B |
| --- | --- | --- | --- |
| None | 64 | 64 | 64 |
| Input | 128 | 0 | 0 |
| Output | 0 | 0 | 128 |
| Energy | 0 | 128 | 0 |
| Chemical | 128 | 0 | 128 |
| Fluid | 0 | 128 | 128 |
| Heat | 128 | 64 | 0 |

## Machine screen

200×176. Used by the twelve processing machines, the modification station, and the heat, bio, and gas generators.

Required widgets:

- Energy gauge at (8, 24), 16×52.
- Eight upgrade slots at x=174, y=`176 - 150 = 26`, stride 18.
- Redstone, side, and security buttons, 16×16, at (138, 4), (156, 4), and (174, 4).
- Progress widget from the preset.
- The item slots and gauges from the preset. A count of 0 omits that widget.

The redstone button cycles `ignored`, `high`, `low`, `pulse`. The side button opens the side screen. The security button cycles `public`, `private`, `trusted` when the player is the owner, and does nothing for anyone else.

| Preset | Blocks | Inputs | Outputs | Gauges | Progress |
| --- | --- | --- | --- | --- | --- |
| `items` | Enrichment, crusher, smelter | (28, 40) | (80, 40) | None | (52, 44), 24×16 |
| `item_chem_in` | Purification, injection, metallurgic infuser | (48, 40) | (96, 40) | Chemical (28, 24) | (72, 44), 22×16 |
| `dissolution` | Dissolution | (28, 40) | None | Chemical (50, 24), chemical (86, 24) | (28, 78), 74×6 |
| `washer` | Washer | None | None | Fluid (28, 24), chemical (56, 24), chemical (84, 24) | (28, 78), 72×6 |
| `crystallizer` | Crystallizer | None | (80, 40) | Chemical (28, 24) | (52, 44), 24×16 |
| `oxidizer` | Oxidizer | (28, 40) | None | Chemical (80, 24) | (52, 44), 24×16 |
| `infuser` | Chemical infuser | None | None | Chemical (28, 24), (52, 24), (84, 24) | (28, 78), 72×6 |
| `separator` | Electrolytic separator | None | None | Fluid (28, 24), chemical (56, 24), chemical (84, 24) | (28, 78), 72×6 |
| `station` | Modification station | (28, 28) and (28, 50) | (80, 40) | None | (52, 44), 24×16 |
| `fuel` | Heat generator, bio generator | (28, 40) | None | None | (52, 44), 24×16 |
| `gas` | Gas-burning generator | None | None | Chemical (28, 24) | (52, 44), 24×16 |
| `reaction` | Pressurized reaction chamber | Item (28, 28) and the substrate leaves at (28, 50) | None | Fluid (56, 24), chemical in (84, 24), chemical out (112, 24) | (56, 78), 72×6 |

The station's upper input is the gear piece. The lower input is the module item. Gauges are 16×52. The first chemical-infuser gauge and the first separator chemical gauge are inputs. The last gauge in each of those presets is the output.

## Factory screen

Same width, same x positions, and the same required buttons as the machine screen. It adds a 16×16 auto-sort button at (120, 4). Slot counts are 3, 5, 7, and 9.

Each process row `i` from 0 puts item slots and the progress widget at `y = 24 + i * 22`. Gauges stay a single shared set at the preset's gauge position. They are not repeated per row. Image height is `110 + count * 22`.

| Tier | Rows | Height | Inventory Y |
| --- | --- | --- | --- |
| Basic | 3 | 176 | 94 |
| Advanced | 5 | 220 | 138 |
| Elite | 7 | 264 | 182 |
| Ultimate | 9 | 308 | 226 |

The last slot of a row ends 8 pixels above the inventory. `226 - (24 + 8 * 22 + 18) = 8`.

## Small screens

176×166 unless a row below says otherwise. Inventory Y is 84. Hotbar Y is 142. No upgrade column. Side, redstone, and security buttons are at (152, 18), (152, 36), and (152, 54), each 16×16, on every small screen that belongs to a block.

| Screen | Required widgets |
| --- | --- |
| Chemical tank and fluid tank | Gauge (48, 18) 16×52. Fill slot (26, 36). Drain slot (116, 36). Dump button (80, 36) 18×18, cycling `none`, `excess`, `all`. |
| Energy cube | Gauge (80, 18) 16×52. Text for stored joules, capacity, and last-tick insert and extract. No item slots and no upgrades. |
| Charge pad | One slot (79, 35). Gauge (48, 18) 16×52. The rate text is 1,000 J/t. |
| Induction matrix | Gauge (8, 18). Text panel (32, 18) 110×52: formed flag, cell count, provider count, stored energy, capacity, transfer rate. |
| Turbine | Gauge (8, 18). Text panel (32, 18): steam mB/t, joules per tick, blades, coils, vents, limiter. |
| Boiler | Gauges at (28, 18), (64, 18), and (100, 18): water, steam, heat. |
| Evaporation | Gauges at (40, 18) and (80, 18): brine, lithium. Text for height and efficiency. |
| SPS | Gauge (8, 18) for energy. Chemical gauges (48, 18) and (80, 18). Text for coil count and progress. |
| Fission | Gauges (8, 18) heat and (40, 18) coolant. Text for fuel, waste, and insertion. Minus button (96, 36). Plus button (114, 36). Each click changes insertion by 1, clamped to 0 through 100. |
| Fusion | Gauges (28, 18), (64, 18), and (100, 18): deuterium, tritium, energy. Text for `idle`, `charging`, `holding`, or `running`. |

A waste barrel has no screen. Right-click does not open one. An induction cell item and an induction cell block have no screen. The matrix screen is the aggregate. The multiblock data panel is the text panel on these structure screens. It is not a second screen.

## QIO dashboard

252×222. Search box (8, 8), 144×16. Item grid (8, 28), 8 columns by 6 rows, stride 18. That is 48 visible types. Scrollbar (152, 28), 6×108. Summary panel (164, 8), 80×120: frequency name, item count, count cap, type count, type cap, security mode. Inventory Y is 140.

Click and search behavior stay in [QIO_GUI.md](QIO_GUI.md). The grid is a window onto the frequency. The scrollbar moves the window by one row.

## Gear screens

The modification station uses the machine preset `station`.

The chest screen is 176 wide and 258 tall. Four columns at `x = 8 + column * 36`, eight rows at `y = 28 + row * 18`. Column headers at y=12 are helmet, chest, legs, boots. That is 32 cells. A cell shows the module icon, its level, and a 16×16 enabled toggle. The chest header tooltip shows stored joules and capacity. Inventory Y is 176.

The tool screen is the same height and uses one column at x=32, eight cells. Sneak-use on the chest item or the worn chest opens the chest screen. Sneak-use on the tool opens the tool screen. Those screens toggle `enabled`. They do not install or remove a module. Install and remove stay on the station, from [GEAR_MODULE_SYSTEM.md](GEAR_MODULE_SYSTEM.md).

## Security desk

Width 176. Height 228. Inventory Y is 146.

| Widget | Position |
| --- | --- |
| Public, Private, Trusted | (8, 18), (62, 18), (116, 18), each 50×16 |
| Name field | (8, 40), 120×16 |
| Add | (132, 40), 36×16 |
| Five list rows | (8, 62 + i * 16), 140×14 |
| Remove on each row | (150, 62 + i * 16), 18×14 |

There is no Apply button. A click applies on that tick. The desk stores the owner's trusted-name list. A trusted-mode block with that owner reads the same list. Five rows are visible. The list scrolls. Add resolves a name to a UUID. An unknown name leaves the list unchanged and shows `gui.mekanika.security.unknown`: "Unknown player."

## Side screen

176×110. No player inventory. Six 18×18 face buttons:

| Face | Position |
| --- | --- |
| Up | (79, 16) |
| North | (79, 36) |
| West | (59, 56) |
| Down | (79, 56) |
| East | (99, 56) |
| South | (79, 76) |

Each face button draws `textures/block/side_overlay.png` tinted with the mode color. Five tabs at y=96, 30×14, x=`8 + index * 32`, in order: item, fluid, chemical, energy, heat. A tab the block does not support is drawn and does not accept a click. A face click cycles that face's mode for the selected tab. Transmitter tabs use `normal`, `push`, `pull`, `none`. Machine tabs use `NONE`, `INPUT`, `OUTPUT`, `BOTH`. The screen opens from the side button. The configurator in the world still cycles the looked-at face and does not open this screen.

## Worked-example index

```normative-examples
example.machine.width=200
example.machine.height=176
example.machine.inv_y=94
example.machine.hotbar_y=152
example.machine.upgrade_y=26
example.machine.upgrade_span=144
example.factory.basic.height=176
example.factory.advanced.height=220
example.factory.elite.height=264
example.factory.ultimate.height=308
example.factory.ultimate.inv_y=226
example.factory.gap=8
example.small.inv_y=84
example.small.hotbar_y=142
example.qio.visible=48
example.qio.width=252
example.qio.height=222
example.qio.inv_y=140
example.gear.cells=32
example.gear.height=258
example.gear.inv_y=176
example.tool.cells=8
example.security.height=228
example.security.inv_y=146
example.security.rows=5
example.side.faces=6
example.side.tabs=5
example.side.height=110
example.frame.center=1
example.screens=17
example.waste_screen=0
example.color.input.r=128
```
