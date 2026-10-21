# Item Transport System (Logistical Transporters)

## Goals
- Reliable item movement with filtering, sorting, and pathfinding.
- Color channels for selective routing.
- Integration with side configuration and machines.

## Core Concepts
- Logistical Transporters move item stacks as discrete units.
- Color coding for channel isolation.
- Restrictive Transporters limit what can enter.
- Diversion Transporters allow routing decisions.
- Logistical Sorter for advanced filtering (mod, modid, tags, NBT).
- Speed upgrades affect travel rate.

## Pathfinding
- Prefer shortest or priority-based paths.
- Support for pull mode from inventories.
- Avoid loops via visited-path tracking.

## Integration
- Side configuration controls which faces accept or emit items.
- Factories and machines expose item handlers.
- Debug tool shows path and filter status.

## Edge Cases
- Full inventories causing backups.
- Rapid filter changes.
- Chunk boundaries.
- Mixed-color networks.
