# COMMAND

Trigger `COMMAND`. Effects none. Goal: explain Agent OS commands from min model.

Do: say commands are Markdown contracts; lifecycle = exact trigger -> lazy-load matching file -> bind/validate inputs -> check permissions/effects -> execute allowed behavior -> return required output or blocker. Mention exact/case-sensitive default, explicit side-effect permission, and path authority: Agent OS resources resolve from loaded OS root while target work resolves from host workspace unless explicitly redirected.

Return:

```text
Agent OS commands are Markdown-defined instructions for repeatable agent behavior.

They work by:
- Matching an exact trigger.
- Loading the matching command file only when needed.
- Binding and validating inputs before execution.
- Executing only permitted behavior and side effects.
- Returning the command's required output or a clear blocker.

Important constraints:
- Command Markdown is the source of truth.
- Commands are case-sensitive and exact by default.
- Agent OS resource paths and host workspace paths are separate authorities.
- Side effects require explicit command permission and active tool permission.
```
