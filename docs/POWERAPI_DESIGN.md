# PowerAPI Design Notes

## Energy Unit

- Name: Joules (or project-chosen name)
- Type: long-based storage
- Transfer model: push-based preferred, with clear capacity and rate limits

## Loader Strategy

- **Fabric**: Full custom implementation (lookup or capability equivalent)
- **Forge / NeoForge**: Bridge to native energy systems or shared internal implementation for consistency

## Requirements

- Consistent across all modules
- Configurable rates and capacities
- Efficient network handling for cables
- Clear documentation of insertion/extraction rules
