# QIO Dashboard GUI

Pixel positions, screen sizes, and required widgets are in [GUI_LAYOUTS.md](GUI_LAYOUTS.md).

## Goals
- Browse and interact with frequency storage.
- Support filtering, security, and importer/exporter configuration.

## Layout
- Item grid or search interface for stored items.
- Frequency indicator.
- Drive capacity and type summary.
- Importer/Exporter status if linked.
- Security mode display.

## Widgets
- Search bar and item list.
- Capacity gauges per drive type.
- Security indicator.
- Filter configuration for importers/exporters.

## Interaction
- Click takes one item. Shift-click takes up to the item's max stack. Shift-click from the player inventory inserts what the frequency accepts.
- Search is a case-insensitive name substring. A query that starts with `#` matches a tag.
- A failed security check closes the screen. The rules are in [QIO_SYSTEM.md](QIO_SYSTEM.md).

## Integration
- Frequency-based data.
- Config cards can copy frequency and security.
- Works while chunks are unloaded where practical.
