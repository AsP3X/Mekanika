# Texture and Model Handoff

What Claude delivers and where Grok puts it. The checklist in [TEXTURE_HANDOFF.md](TEXTURE_HANDOFF.md) still applies to every file. This file is the list.

## Normative constants

```normative
texture.size=16
texture.factory_overlays=4
texture.processing_machines=12
texture.factory_models=48
texture.tier_colors=4
model.proxy=empty
```

Every block and item texture is a 16×16 PNG. Tier colors are basic green, advanced red, elite cyan, and ultimate purple. Designs are original industrial art. GUI art is `textures/gui/frame.png` (9×9) and `textures/gui/widgets.png` (256×256), with the rectangles in [GUI_LAYOUTS.md](GUI_LAYOUTS.md).

## Paths

| Kind | Path |
| --- | --- |
| Block model | `assets/mekanika/models/block/<id>.json` |
| Item model | `assets/mekanika/models/item/<id>.json` |
| Unique block texture | `assets/mekanika/textures/block/<id>.png` |
| Unique item texture | `assets/mekanika/textures/item/<id>.png` |
| Factory overlay | `assets/mekanika/textures/block/factory_overlay_<tier>.png` |

A block item uses the block model unless the row below says the item has its own texture. Glass and ports use render type cutout. `bounding_proxy` has an empty model and no texture.

## Shared textures

The twelve processing machines in [MACHINE_IO.md](MACHINE_IO.md) each have one texture, `<id>.png`, with a two-frame active front. Frame 2 plays while status is `running`. The 48 factory models reuse that texture plus `factory_overlay_<tier>.png`. They do not each get a new painting.

Tiered cables, pipes, tubes, conductors, energy cubes, chemical tanks, and fluid tanks each get one texture per tier. The tier color is the casing color. Fluid tank ids are in [BUFFERED_NETWORKS.md](BUFFERED_NETWORKS.md).

## Exceptions already specified

Elite and ultimate solar generators and every wind generator use the oversized models and proxy blocks in [GENERATORS_REACTORS.md](GENERATORS_REACTORS.md). Those model files are still `models/block/<id>.json`. Their textures are 16×16 sheets, not a scaled-up painting.

Multiblock casings, valves, glass, and interior blocks use the ids and English names in the registry table of [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md). Each of those ids has one texture. Controllers add a two-frame front, same rule as machines.

## Side overlay

`textures/block/side_overlay.png` is a 16×16 sheet with one pixel frame. The side-config screen tints it. It is not a block texture.

## Worked-example index

```normative-examples
example.texture.size=16
example.processing_textures=12
example.factory_models=48
example.factory_overlays=4
example.proxy_textures=0
```
