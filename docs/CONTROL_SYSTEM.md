# Control System API — Clean-Room Design

This document defines the conceptual design for Mekanika’s control systems, including redstone control, security modes, computer/peripheral integration, and the Configurator tool. All content is original.

## Overview

Machines and structures support multiple control layers:
- Redstone control (activation modes).
- Security modes (access control).
- Computer methods (for CC: Tweaked or similar).
- Configurator tool for side configuration and other modes.
- Logic adapters for advanced multiblock control.

## Redstone Control

- Modes:
  - Disabled / Ignored: Machine always runs (if other conditions met).
  - High / Normal: Runs when redstone signal is present.
  - Low / Inverted: Runs when no redstone signal is present.
  - Pulse: Runs for one operation or short duration on rising edge.
- Stored per machine in a component.
- GUI tab or button to cycle modes.
- Comparator output based on fill level, progress, or status.
- Logic adapters on multiblocks expose specific signals (e.g., high temperature, excess waste, damage critical).

## Security Modes

- Modes:
  - Public: Anyone can access.
  - Private: Only the owner can access.
  - Trusted: Owner and players on the trusted list can access.
- Owner is set on placement.
- Trusted players are managed via a Security Desk.
- Operators can bypass security (configurable).
- Unauthorized players cannot open GUIs, break the machine (contents may be protected), or interact with it.
- GUI lock icon shows and allows cycling modes (owner only).

## Computer / Peripheral Integration

- Machines and multiblocks expose methods for computers (e.g., CC: Tweaked).
- Common methods:
  - getEnergy(), getProgress(), getStatus().
  - activate() / deactivate() / scram().
  - getTemperature(), getDamagePercent(), getCoolant() (for reactors).
  - setRate(), getBurnRate().
- Methods are annotated or registered for automatic exposure.
- Support for both direct and modem connections.
- Error handling for invalid calls or security restrictions.

## Configurator Tool

- Multi-mode tool:
  - Configurate: Cycle side configuration modes.
  - Wrench: Rotate machines.
  - Optional: Network reader, security, etc.
- Right-click reports current mode; Shift + right-click cycles.
- Works on machines, transmitters, and multiblock ports.
- Active transmission type can be selected.

## Logic Adapters

- Special blocks for multiblocks (reactors, turbines, etc.).
- Configurable to emit or receive redstone based on conditions:
  - Activation / scram.
  - High temperature.
  - Excess waste.
  - Damage critical.
  - Insufficient fuel.
- Integrate with computer methods for advanced automation.

## Implementation Recommendations

1. Implement redstone control as a reusable component early.
2. Add security modes with owner tracking and trusted list support.
3. Expose a clean set of computer methods for key machines and structures.
4. Make the Configurator a multi-mode tool from the start.
5. Support logic adapters for nuclear and large structures.
6. Document all computer methods clearly for players and addon authors.
7. Rate-limit computer method calls if needed to prevent lag.
8. Ensure security checks are applied to computer methods and GUI access.

## Edge Cases

- Pulse mode with rapid redstone signals.
- Security changes while a player has a GUI open.
- Computer methods called on unformed multiblocks.
- Trusted list updates not immediately reflected.
- Operator bypass configuration.
- Logic adapter signals during structure formation/invalidation.

## Player Feedback

- Clear GUI indicators for redstone control and security modes.
- Messages when access is denied.
- Debug tool shows control state and last computer method calls (optional).
- Status lights or comparator output for key conditions.

This control system provides flexible automation and access control while remaining consistent across machines and structures.
