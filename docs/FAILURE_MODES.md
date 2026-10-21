# Failure Mode Catalog

## Multiblocks
- Invalid structure → clear message + particle feedback.
- Missing required components → GUI indicator.
- Overheating → damage or shutdown with warning.

## Radiation
- High exposure without protection → damage + visual/audio warning.
- Geiger counter for detection.

## Networks
- Full buffer → backpressure or drop with feedback.
- Disconnected section → debug tool shows isolation.

## Machines
- Insufficient energy/chemicals → status message in GUI.
- Redstone disabled → visual indicator.
- Security denied → clear access message.

## Player Feedback
- All failure modes should have both GUI text and optional particle/sound feedback.
- Debug tool reports last failure reason.
