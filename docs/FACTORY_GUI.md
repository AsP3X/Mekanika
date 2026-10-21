# Factory GUI

Pixel positions, screen sizes, and required widgets are in [GUI_LAYOUTS.md](GUI_LAYOUTS.md).

## Goals
- Display multiple parallel process slots clearly.
- Shared energy and chemical displays.
- Auto-sort toggle and progress feedback.

## Layout
- One row per process slot. Counts are 3, 5, 7, and 9. Positions are in the layout doc.
- Shared energy gauge and chemical/fluid tanks.
- Auto-sort toggle button.
- Upgrade slots.
- Side configuration access.

## Widgets
- One progress indicator per slot. The rows are in [GUI_LAYOUTS.md](GUI_LAYOUTS.md).
- Shared energy, chemical, and fluid gauges.
- The auto-sort button toggles `mekanika:auto_sort`. The tick rule is in [FACTORIES.md](FACTORIES.md).

## Interaction
- Same as Machine GUI plus auto-sort control.
- Ingredients can be placed in any compatible slot.
- Configurator works as on single machines.

## Integration
- Reuses machine widgets.
- Tracks progress per slot.
- Debug tool shows active slot efficiency.
