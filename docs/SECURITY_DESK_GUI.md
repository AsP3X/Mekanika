# Security Desk GUI for Mekanika

Pixel positions, screen sizes, and required widgets are in [GUI_LAYOUTS.md](GUI_LAYOUTS.md).

This document defines the Security Desk and trusted list management interface. All content is original and conceptual.

## Goals

- Allow owners to manage access to machines, tanks, QIO frequencies, and multiblocks.
- Support Public, Private, and Trusted modes.
- Provide a clear way to add and remove trusted players.
- Integrate with the Control System.

## Layout

- Mode selector: Public / Private / Trusted.
- Trusted player list with names.
- Input field to add a player by name.
- Remove button per entry.
- Add and remove apply on that tick. There is no Apply button.
- An unknown name shows "Unknown player." and leaves the list unchanged.

## In-Machine Compact Panel

- Many machines include a security button.
- Shows current mode and owner.
- Allows mode changes if the player is the owner.
- Link to the full Security Desk for trusted list management.

## Trusted List Behavior

- Names are resolved to UUIDs.
- List is persisted with the block or frequency.
- Changes apply immediately.
- Unauthorized players cannot modify the list.

## Interactions

- Only the owner can change settings.
- Unauthorized access shows a clear denial message.
- Config cards copy mode but not ownership or the full trusted list by default.

## Integration

- Uses the security component from the Control System.
- Applies to machines, tanks, QIO, and multiblock controllers.
- Debug tool reports owner and mode.
- Creative mode bypass is `general.creative_bypass`, default false. Operator bypass is `general.operator_bypass`, default false. Both are in [CONTROL_SYSTEM.md](CONTROL_SYSTEM.md).

## Edge Cases

- Owner offline.
- Name resolution failure.
- Trusted list changes while another player has the GUI open.
- Security on ports vs controller.

## Implementation Notes

- Validate permissions server-side.
- Keep the desk GUI simple.
- Provide clear feedback messages.
- Document the default mode.

The Security Desk enables multiplayer use without complexity.
