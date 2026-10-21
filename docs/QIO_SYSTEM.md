# QIO / Frequency Storage System

## Goals
- Wireless, frequency-based item storage.
- Drive arrays with type and capacity limits.
- Security modes and remote access.

## Core Concepts
- Frequency: unique ID linking dashboards, drives, and importers/exporters.
- Drive Array: holds drives with type limits and capacity.
- Dashboard: GUI for browsing and interacting with the frequency.
- Importer/Exporter: automated item transfer with filters.
- Security: public, private, trusted modes.

## Behavior
- Works while chunks are unloaded (within reason).
- Drive type limits prevent single-item overflow.
- Config cards can copy frequency and security settings.
