# Configurator and Side Configuration GUI

## Goals
- Visual and interactive side configuration.
- Clear color language for all transmission types.
- Support for cycling modes and bulk operations.

## Layout
- 3D or unfolded face representation of the block.
- Color-coded faces matching transmission types.
- Mode legend.
- Transmission type selector.
- Current mode display for hovered face.

## Widgets
- Clickable faces.
- Color overlays (grey=none, dark red=input, dark blue=output, green=energy, etc.).
- Mode cycle feedback.

## Interaction
- Opened via Configurator or button in machine GUI.
- Click or shift-click to cycle modes.
- Works on machines and transmitters.
- Visual feedback when Configurator is held in the world.

## Integration
- Shared side configuration system.
- Triggers network rebuild requests.
- Config cards can copy the resulting configuration.
