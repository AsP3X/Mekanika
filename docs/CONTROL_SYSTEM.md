# Control System API Design for Mekanika

This document provides detailed, clean-room design guidance for machine control systems, including redstone control, security, the Configurator tool, and config cards. All content is original and conceptual.

## Goals

- Provide consistent control options across machines, transmitters, and storage.
- Support redstone-based automation.
- Enable ownership and access control for multiplayer.
- Make configuration fast and copyable.
- Integrate cleanly with side configuration and networks.
- Remain lightweight and data-driven where possible.

## Redstone Control

### Modes
- Disabled (always on)
- Low (active when no redstone signal)
- High (active when redstone signal present)
- Pulse (active for one tick on signal change)

### Behavior
- Machines check redstone mode before processing.
- Transmitters can optionally respect redstone for push/pull.
- Multiblock controllers can expose redstone control.
- Comparator output based on fill level, progress, or status.

### Implementation
- Store redstone mode in a component or attachment.
- Update on neighbor redstone changes.
- Support both direct redstone and comparator input.

## Security and Ownership

### Modes
- Public (anyone can access)
- Private (owner only)
- Trusted (owner + trusted players)

### Features
- Ownership set on placement (player who placed the block).
- Trusted list managed via Security Desk or in-GUI.
- Applies to machines, tanks, bins, QIO, and multiblocks.
- Security can be copied via config cards.

### Implementation
- Store owner UUID and trusted list.
- Check security before opening GUI or allowing automation.
- Optional security override for operators or creative.

## Configurator Tool

### Modes
- Configure (cycle side modes)
- Wrench (rotate, dismantle)
- Empty (or other utility modes)

### Behavior
- Shift + right-click cycles the mode for the hovered transmission type.
- Right-click reports current mode or opens side config GUI.
- Works on machines and transmitters.
- Visual feedback (colored overlays or particles) when held.
- Can switch active transmission type (energy, item, fluid, chemical, heat).

### Integration
- Triggers network rebuild requests when connection modes change.
- Compatible with config cards for bulk operations.

## Config Cards

### Features
- Copy side configuration, redstone mode, security settings, and upgrades.
- Paste onto compatible machines.
- Optional factory-aware pasting.
- Feedback on success or mismatch.

### Implementation
- Store copied data as a serializable data component or NBT.
- Validate compatibility before applying.
- Support batch paste on multiple machines.

## Side Configuration Integration

- Side configuration is the primary control for resource flow.
- Redstone and security are orthogonal but complementary.
- Configurator is the main tool for interacting with side config.
- All control settings should be serializable for config cards and NBT.

## Multiblock Considerations

- Controllers expose redstone and security settings.
- Ports can have independent side configuration.
- Security is checked at the controller level.
- Formation success can be signaled via redstone or particles.

## Debug and Player Feedback

- Debug tool reports current redstone mode, security owner, and side config summary.
- GUI shows redstone and security status.
- Clear messages when access is denied due to security.
- Configurator provides immediate visual feedback.

## Implementation Recommendations

1. Implement redstone control early (simple and high value).
2. Add security after basic machines are working.
3. Build the Configurator as a multi-mode tool from the start.
4. Support config cards once side config is stable.
5. Integrate all control systems with the shared machine base.
6. Make modes data-driven where practical.
7. Add debug visibility for all control states.

## Edge Cases

- Rapid redstone pulsing.
- Security changes while a player has the GUI open.
- Config cards applied to incompatible machines.
- Multiblock ports with conflicting redstone settings.
- Chunk unload with active redstone control.
- Trusted list management in multiplayer.
- Configurator used on unformed multiblocks.

## Balance and UX

- Redstone control should feel responsive.
- Security should not interfere with single-player or private worlds by default.
- Configurator should be craftable early and useful throughout the game.
- Config cards are a mid-to-late quality-of-life feature.

This design provides a cohesive control system that supports both automation and multiplayer use while remaining consistent across all machine types.
