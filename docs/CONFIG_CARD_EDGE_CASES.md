# Config Card Edge Cases for Mekanika

This document details expected behavior when using config cards. All content is original and conceptual.

## Goals

- Define clear, predictable paste behavior.
- Handle mismatches gracefully without data loss.
- Provide actionable player feedback.
- Support partial application where possible.

## What Is Copied

- Side configuration for all supported transmission types.
- Redstone control mode.
- Security mode (not ownership or trusted list).
- Upgrade configuration (compatible slots only).
- Auto-eject and related flags.
- Factory shared settings.
- Transmitter color and filter settings where applicable.

## Edge Cases and Behavior

### Incompatible Transmission Types
- Apply only supported types.
- Skip unsupported types and report them in chat or action bar.
- Example: chemical settings pasted onto a machine without chemical support are ignored.

### Upgrade Mismatches
- Fewer slots on target: fill available slots, report overflow.
- Unsupported upgrade type: skip and notify.
- Do not remove existing upgrades unless explicitly designed.

### Security and Ownership
- Ownership is never copied.
- Security mode is copied only if the player has permission on the target.
- Trusted lists are not copied by default.

### Factory vs Single-Slot
- Factory → single: apply shared side config and redstone; ignore slot-specific data.
- Single → factory: apply shared settings to the factory.

### Multiblock Ports
- Copy port side configuration.
- Incompatible port types skip unsupported settings.

### Partial Failure
- Always apply what is possible.
- Report skipped items clearly.
- Do not roll back successful changes.

### Invalid or Corrupted Data
- Reject the paste with a clear message.
- Do not modify the target.

## Player Feedback

- Full success: brief confirmation.
- Partial success: list of skipped settings.
- Failure: reason and no changes.
- Debug tool can inspect card contents.

## Implementation Recommendations

1. Use a versioned serialization format.
2. Validate compatibility before applying.
3. Apply in a defined order (side config → redstone → upgrades).
4. Provide detailed feedback messages.
5. Test cross-type pastes early.
6. Document supported fields per machine or block type.

## Edge Case Summary

- Different supported transmission types.
- Upgrade slot or type mismatches.
- Security permission issues.
- Factory vs non-factory.
- Multiblock vs single block.
- Corrupted or outdated card data.
- Rapid successive pastes.

Clear edge-case handling makes config cards reliable for automation setups.
