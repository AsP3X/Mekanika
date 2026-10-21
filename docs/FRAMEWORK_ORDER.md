# Framework Implementation Order — Mekanika

Recommended order to minimize rework:

1. **PowerAPI** — Energy unit, containers, basic transfer, Fabric implementation + bridges.
2. **Chemical system** — Chemical, Stack, Tank, Handler interfaces.
3. **Machine base** — Shared tile with energy, side config component, upgrades, redstone, security.
4. **Multiblock framework** — FormationProtocol, validators, managers, data, caches, invalidation.
5. **Transmitter base** — Network formation, capacity/buffer, per-resource networks.
6. **Configurator** — Side config interaction tool.
7. **Debug tools** — Network reader / structure status command.
8. **Basic machines** — Enrichment, Crusher, Smelter, Infuser using the base.
9. **Factories** — Parallel slots reusing recipe logic.
10. **Processing multiblocks** — Dynamic Tank, Evap, Boiler.
11. **Generators and Storage** — Simple generators, then Turbine/Fission/Fusion, Induction Matrix, QIO.
12. **Tools/Gear** — Modular system.

## Notes
- Data-driven recipes and configs from the start.
- JEI/EMI integration as machines are added.
- Test formation, networks, and chemical transfer early.
