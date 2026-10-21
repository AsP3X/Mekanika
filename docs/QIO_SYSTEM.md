# QIO / Frequency Storage System

## Goals
- Wireless, frequency-based item storage with large capacity.
- Drive arrays with type and capacity limits.
- Security modes and remote access.

## Core Concepts
- Frequency: unique ID linking dashboards, drives, importers, and exporters.
- Drive Array: holds drives with type limits and total capacity.
- Dashboard: GUI for browsing, inserting, and extracting from the frequency.
- Importer/Exporter: automated item transfer with filters and rate limits.
- Security: public, private, trusted modes.

## Behavior
- Works while chunks are unloaded (within reason).
- Drive type limits prevent single-item overflow.
- Config cards can copy frequency and security settings.
- Dashboard access respects security.

## Edge Cases
- Frequency collisions.
- Drive array overload.
- Chunk unload during transfer.
- Security changes while GUI is open.
