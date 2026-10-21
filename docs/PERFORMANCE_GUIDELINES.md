# Performance Guidelines

## Tick Methods
- Avoid allocations in tick paths.
- Rate-limit expensive checks (formation, network rebuilds).
- Use dirty flags so unchanged structures skip work.

## Networks
- Maintain acceptor/emitter lists.
- Batch transfers.
- Limit rebuilds per tick.

## Multiblocks
- Fail-fast validation.
- Cache aggregated stats.
- Prefer block-state checks over tile lookups.

## General
- Profile large factories and max-size structures early.
- Use long arithmetic for energy and chemical amounts.
- Prefer data-driven values over hard-coded constants.
