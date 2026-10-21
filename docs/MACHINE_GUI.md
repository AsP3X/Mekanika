# Machine GUI for Mekanika

This document defines the standard GUI for processing machines. All content is original and conceptual.

## Goals

- Consistent layout across all single-slot processing machines.
- Clear input, output, progress, energy, and chemical/fluid displays.
- Easy access to side configuration, upgrades, redstone, and security.
- Support JEI/EMI ghost ingredients.

## Layout

- Player inventory at the bottom.
- Input slots on the left.
- Output slots on the right.
- Progress bar or arrow in the center.
- Energy bar on the left or top.
- Chemical/fluid tank widgets near relevant slots.
- Upgrade slots in a dedicated area (top or side).
- Side config, redstone, and security buttons in fixed positions.
- Extra slots (catalyst, secondary input) clearly labeled.

## Widgets

- Energy bar with tooltip (exact energy / capacity).
- Progress bar with remaining time tooltip.
- Tank gauges with type, amount, and color.
- Status icons for no energy, redstone paused, or missing inputs.
- Upgrade slot tooltips explaining effects.

## Interactions

- Right-click opens GUI.
- Shift-click for quick transfer.
- Ghost ingredients from JEI/EMI into input slots.
- Configurator can open side config or cycle modes.
- Buttons cycle redstone mode or open security.

## Integration

- Uses shared machine base data.
- Side configuration drives capability access.
- Syncs progress, energy, and tank levels (rate-limited).
- Config cards can copy settings.

## Edge Cases

- Missing secondary inputs.
- Energy depletion mid-process.
- Multiple tanks.
- Very long processing times.

## Implementation Notes

- Extend a shared machine screen base.
- Reuse energy, progress, and tank widgets.
- Document slot coordinates for texture handoff.

This pattern applies to Enrichment Chamber, Crusher, Smelter, Purification Chamber, and similar machines.
