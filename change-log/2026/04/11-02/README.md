# Change Log - 2026-04-11 02

## Summary

This update tightens the `COMPILE` command so it derives compiled commands only from direct child Markdown files under `os/commands`. The compiled minimal OS now includes `ADD_CHANGE_LOG` in its lazy registry, and the Agent OS semantic version moves to `1.0.0` because the recursive command discovery behavior was intentionally removed.

## Changes

- `os/commands/COMPILE.md`: Changes command discovery from recursive `os/commands/**/*.md` scanning to direct child `os/commands/*.md` scanning, explicitly excluding `os/commands/Invariants.md` and avoiding hard-coded default commands.
- `os/min/BOOT.md`: Adds `ADD_CHANGE_LOG` to the minimal OS lazy registry so the compact boot path can dispatch the changelog workflow.
- `os/min/commands/COMPILE.md`: Mirrors the direct-child command discovery rule for the compiled `COMPILE` command and removes the fixed generated command list.
- `os/min/commands/ADD_CHANGE_LOG.md`: Adds the compiled, token-lean `ADD_CHANGE_LOG` command that can create dated changelog entries and bump the semantic version line.
- `os/BOOT.md`: Increments the semantic version from `0.2.0` to `1.0.0`.

## Git Status

```text
 M os/commands/COMPILE.md
 M os/min/BOOT.md
 M os/min/commands/COMPILE.md
?? os/min/commands/ADD_CHANGE_LOG.md
```
