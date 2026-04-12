# COMPILE

Trigger `COMPILE`. Audience LLM. Goal: regenerate `os/min` with minimum tokens while preserving behavior.

Inputs default: `source_root=os`, `output_root=os/min`. Effects allowed: create/replace/remove only inside `os/min`; no source edits outside `os/min`; no Git/system/network changes.

Do:
1. Read source docs from `os` excluding `os/min`; discover direct child `os/commands/*.md` only.
2. Emit `os/min/BOOT.md` only for boot: identity/dispatch/safety/effects/cache/lazy-load rules.
3. Emit compact self-sufficient command docs under `os/min/commands`; `BOOT` maps to `os/min/BOOT.md`.
4. Optimize for LLM token economy: terse rules, tables/lists, no examples/tutorial/decorative/redundant text unless needed for equivalent execution.
5. Preserve semantics; change wording/layout/file boundaries to reduce tokens.
6. Max lazy loading: boot has registry+global rules; command specifics stay in command files loaded after exact trigger.
7. Remove stale generated files under `os/min` not in current set; touch nothing outside `os/min`.

Generated set rule:

```text
os/min/BOOT.md
os/min/commands/<direct-command-name>.md
```

Derive commands dynamically from `os/commands/*.md`; exclude `Invariants.md`; do not scan nested dirs; do not hard-code default commands.

Return:

```text
COMPILE complete.

Generated:
- <path>

Removed:
- <path or "none">

Boot:
- /os/min/BOOT.md

Notes:
- Target is LLM context: boot loads only os/min/BOOT.md; command files load lazily after exact trigger match and may be cached for the session.
```
