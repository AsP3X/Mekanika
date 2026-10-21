# Transmitter Configuration for Mekanika

This document defines configuration options for transmitters (cables, pipes, tubes, transporters). All content is original and conceptual.

## Goals

- Allow players to control connections, colors, and filters.
- Integrate with the Configurator tool.
- Support networks without requiring a full GUI for every cable.
- Remain consistent with side configuration where applicable.

## Configuration Options

### Connection Mode
- Normal, Push, Pull, or None per face (where supported).
- Controlled via Configurator or side configuration.

### Color Channels (Item Transporters)
- Multiple colors to separate item streams.
- Configurable via Configurator or small GUI.
- Mismatched colors do not connect.

### Filters
- Item or tag filters on transporters and sorters.
- Configurable via Configurator or GUI.
- Applied at network or segment level.

### Tier and Throughput
- Determined by the transmitter block itself.
- Mixed tiers in one network use the limiting tier or defined rules.

## Configurator Integration

- Cycle connection modes.
- Set color.
- Open filter GUI if present.
- Wrench mode for rotating or removing.

## Integration

- Uses the shared networking system.
- Side configuration on connected machines interacts with transmitter faces.
- Debug tool reports color, filter, and connection status.
- Config cards can copy transmitter settings where applicable.

## Implementation Notes

- Keep per-cable data minimal.
- Sync only changed settings.
- Document default connection behavior.
- Test mixed-tier and color networks.

## Edge Cases

- Color changes while items are in transit.
- Filter updates mid-transfer.
- Disconnected segments.
- Creative mode infinite throughput.

Transmitter configuration enables precise automation without excessive GUIs.
