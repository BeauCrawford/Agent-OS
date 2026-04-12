# Change Log - 2026-04-11 01

## Summary

This update expands Agent OS with a stronger global operating contract and a new changelog workflow. Full boot now loads OS-level invariants, and agents can use `ADD_CHANGE_LOG` to turn current repository deltas into dated, user-friendly changelog entries while bumping the Agent OS semantic version.

## Changes

- `os/BOOT.md`: Loads `os/Invariants.md` during full boot, includes it in the boot output contract, and increments the semantic version from `0.1.0` to `0.2.0`.
- `os/Invariants.md`: Adds OS-level invariants in self-documenting `UNIVERSAL_ID` format, covering identity, document authority, boot behavior, dispatch, side effects, compilation, context handling, output, safety, privacy, and failure behavior.
- `os/commands/ADD_CHANGE_LOG.md`: Adds a side-effecting command that runs `SHOW_DELTAS`, creates the next `change-log/YYYY/MM/DD-NN/README.md` entry when deltas exist, and updates the semantic version in `os/BOOT.md`.

## Git Status

```text
 M os/BOOT.md
?? os/Invariants.md
?? os/commands/ADD_CHANGE_LOG.md
```
