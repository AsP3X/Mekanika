# Localization for Mekanika

This document defines naming and language key conventions. All content is original and conceptual.

## Goals

- Consistent, clear names for all blocks, items, and messages.
- Support for multiple languages later.
- Easy for Claude and Grok to follow.
- No trademarked names from other mods.

## Key Prefixes

- Blocks: `block.mekanika.<id>`
- Items: `item.mekanika.<id>`
- Tooltips: `tooltip.mekanika.<id>`
- Messages: `message.mekanika.<category>.<id>`
- GUIs: `gui.mekanika.<screen>.<element>`
- Configs: `config.mekanika.<key>` (optional)

## Naming Style

- Clear, descriptive, industrial tone.
- Tier names: Basic, Advanced, Elite, Ultimate.
- Avoid exact copies of other mods’ names.
- Consistent capitalization.

## Examples

- `block.mekanika.enrichment_chamber` → "Enrichment Chamber"
- `item.mekanika.infused_alloy` → "Infused Alloy"
- `message.mekanika.multiblock.invalid_shape` → "Invalid structure shape"
- `gui.mekanika.side_config.title` → "Side Configuration"

## Implementation Notes

- All user-facing strings use lang keys.
- Provide an English (`en_us`) file as the base.
- Document new keys when adding content.
- Support data pack lang overrides.
- Test for missing translations (fallback to key or English).

## Edge Cases

- Very long names in GUIs.
- Pluralization if needed.
- Dynamic values in messages (use format placeholders).

Consistent localization keeps the mod professional and translatable.
