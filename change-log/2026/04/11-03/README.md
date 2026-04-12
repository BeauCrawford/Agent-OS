# Change Log - 2026-04-11 03

## Summary

This update makes `SHOW_DELTAS` a canonical direct child command under `os/commands`, keeping it discoverable by the new direct-child `COMPILE` rule. The changelog workflow and compiled minimal command set now point to that canonical path, and Agent OS moves to `1.1.0` for the new user-facing command layout.

## Changes

- `os/commands/SHOW_DELTAS.md`: Adds the canonical direct child `SHOW_DELTAS` command so `COMPILE` can discover it from `os/commands/*.md`.
- `os/commands/ADD_CHANGE_LOG.md`: Updates the required `SHOW_DELTAS` dependency from the legacy nested path to `os/commands/SHOW_DELTAS.md`.
- `os/min/commands/ADD_CHANGE_LOG.md`: Updates the compiled changelog command to execute the canonical direct `SHOW_DELTAS` behavior.
- `os/min/commands/SHOW_DELTAS.md`: Notes the canonical source path for the compiled `SHOW_DELTAS` command.
- `os/BOOT.md`: Increments the semantic version from `1.0.0` to `1.1.0`.

## Git Status

```text
 M os/BOOT.md
 M os/commands/ADD_CHANGE_LOG.md
 M os/commands/COMPILE.md
 M os/min/BOOT.md
 M os/min/commands/COMPILE.md
 M os/min/commands/SHOW_DELTAS.md
?? change-log/2026/04/11-02/README.md
?? os/commands/SHOW_DELTAS.md
?? os/min/commands/ADD_CHANGE_LOG.md
```
