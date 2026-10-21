# Debug Tool Specification

## Network Reader / Debug Command

### Multiblock
- Formed status, size (L×W×H), component counts, last failure reason and position.

### Energy Network
- Capacity, current buffer, throughput (current/max), node count, acceptor/emitter counts.

### Chemical / Fluid Network
- Type, amount, flow rate, filter status.

### Item Network
- Color channel, filter summary, path status.

### Control
- Side config summary, redstone mode, security owner.

### Radiation
- Current exposure level if applicable.

## Access
- Gated behind config flag or creative mode.
- Available as both item and command.
