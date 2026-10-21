# Gear and MekaSuit GUI

Pixel positions, screen sizes, and required widgets are in [GUI_LAYOUTS.md](GUI_LAYOUTS.md).

## Goals
- Manage installed modules, energy, and active abilities.
- Clear installation, removal, and conflict feedback.

## Layout
- Module slots with installed modules.
- Energy gauge for the suit or tool.
- Active ability indicators.
- Conflict warnings.
- HUD preview or status.

## Widgets
- Module grid.
- Energy display.
- Toggle or mode buttons for modules.
- Tooltip with energy cost and effects.

## Interaction
- Install and remove modules at the modification station. One type uses one row. A second copy raises the level.
- The chest screen lists every worn MekaSuit piece. The tool screen lists only the tool.
- A refused install shows the conflict or the full-slot reason from [GEAR_MODULE_SYSTEM.md](GEAR_MODULE_SYSTEM.md) and does not consume the item.

## Integration
- Shared energy system.
- Radiation protection modules shown with status.
- Persistence in item data.
