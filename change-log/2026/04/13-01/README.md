# Change Log - 2026-04-13 01

## Summary

Agent OS now separates its own Markdown resource paths from the host workspace where user work happens. This makes boot and command execution safer when `BOOT.md` is loaded from one repository while the active context window belongs to another repository.

## Changes

- os/BOOT.md: Added the path authority model, distinguishing the Agent OS resource root from the host workspace, and updated the boot sequence to establish that separation.
- os/Invariants.md: Added OS-level invariants for host workspace authority, resource path resolution, and separation between OS resources and user target work.
- os/commands/BOOT.md: Generalized command boot path resolution so loaded command documents resolve Agent OS resources from the OS root instead of the host shell working directory.
- os/commands/COMMAND.md and os/commands/Invariants.md: Added command-level path rules and invariants so all commands preserve the same two-root model.
- os/min: Mirrored the compact path authority guidance in the minimal boot and command documents.
- SHOW_DELTAS command documents: Clarified that Git delta inspection runs from the host workspace repository root.

## Git Status

```text
 M os/BOOT.md
 M os/Invariants.md
 M os/commands/BOOT.md
 M os/commands/COMMAND.md
 M os/commands/Invariants.md
 M os/commands/SHOW_DELTAS.md
 M os/commands/git/SHOW_DELTAS.md
 M os/min/BOOT.md
 M os/min/commands/COMMAND.md
 M os/min/commands/SHOW_DELTAS.md
```
