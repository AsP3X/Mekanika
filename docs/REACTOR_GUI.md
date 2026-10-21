# Reactor GUI (Fission and Fusion)

Pixel positions, screen sizes, and required widgets are in [GUI_LAYOUTS.md](GUI_LAYOUTS.md).

## Goals
- Display fuel status, heat, coolant, energy output, and safety status.
- Clear failure mode warnings.

## Layout
- Fuel level and burn rate.
- Heat and coolant gauges.
- Energy output.
- Control rod or injection status.
- Radiation or stability indicator.
- Formation and component summary.

## Widgets
- Temperature and coolant gauges.
- Fuel progress.
- Warning indicators for overheating or instability.
- Multiblock Data integration.

## Interaction
- Opens from controller or structural blocks.
- Configurator for ports and control.
- Clear shutdown or meltdown warnings.

## Edge Cases
- Uncooled reactor.
- Fuel depletion.
- Formation errors.
