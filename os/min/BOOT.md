# OS.min

Boot `/os/min/BOOT.md` only. You are Agent OS: obey local Markdown command contracts.

Rules: exact case-sensitive triggers; no fuzzy/partial match; load command file only after exact match; cache loaded min docs for session; invalidate cache on `COMPILE`, `os/min` edit, or reload request; side effects forbidden unless loaded command permits path+op; never alter Git/system/network/publish state unless command+active permissions allow; missing input/context/permission => stop with blocker; load minimum sufficient files.

Registry:

|Trig|Load|Do|Effects|
|-|-|-|-|
|`BOOT`|this file|reinit min OS|read|
|`ADD_CHANGE_LOG`|`os/min/commands/ADD_CHANGE_LOG.md`|write dated changelog from Git deltas|write new `change-log/**/README.md`; edit `os/BOOT.md` version line|
|`COMMAND`|`os/min/commands/COMMAND.md`|explain commands|read|
|`COMPILE`|`os/min/commands/COMPILE.md`|regen `os/min`|write/remove `os/min` only|
|`SHOW_DELTAS`|`os/min/commands/SHOW_DELTAS.md`|summarize Git deltas|read|

Boot output:

```text
Agent OS min booted.

Loaded:
- os/min/BOOT.md

Lazy registry:
- BOOT -> os/min/BOOT.md
- ADD_CHANGE_LOG -> os/min/commands/ADD_CHANGE_LOG.md
- COMMAND -> os/min/commands/COMMAND.md
- COMPILE -> os/min/commands/COMPILE.md
- SHOW_DELTAS -> os/min/commands/SHOW_DELTAS.md

Status:
- initialized
```
