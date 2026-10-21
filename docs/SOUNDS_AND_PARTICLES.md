# Sounds and Particles for Mekanika

This document defines audiovisual feedback. All content is original and conceptual.

## Goals

- Provide clear, non-intrusive feedback for important events.
- Support performance options.
- Keep assets original.
- Integrate with failure modes and multiblock systems.

## Sounds

- Machine processing loop (subtle, looping while active).
- Multiblock formation success.
- Multiblock formation failure.
- Energy transfer or turbine hum (optional, low volume).
- UI clicks for mode and config changes.
- Reactor warning tones (escalating).
- Radiation Geiger ticks (intensity based on exposure).
- Item insertion/rejection on transporters (light).

## Particles

- Multiblock formation highlight (temporary outline or sparkles).
- Invalidation or error particles at the problem location.
- Processing sparks or steam from active machines.
- Heat or radiation warning particles.
- Subtle cable or transmitter activity indicators.
- Steam or coolant flow visuals on turbines and reactors.

## Configuration

- Client options to reduce or disable particles.
- Sound volume controls.
- Server-side option to limit particle broadcasts if needed.

## Implementation Notes

- Add basic sounds and particles with core machines.
- Expand for multiblocks and reactors.
- Keep particle counts reasonable.
- Document event triggers.
- Test performance with many active machines.

## Edge Cases

- High particle counts in large bases.
- Sound spam from rapid events.
- Client-only vs server-synced effects.

Clear audiovisual feedback improves usability without being required for core function.
