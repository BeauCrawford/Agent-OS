# ADD_CHANGE_LOG

Trigger `ADD_CHANGE_LOG`. Goal: run `SHOW_DELTAS`; if deltas exist, create next user changelog entry and bump `os/BOOT.md` semver.

Effects allowed: create `change-log/YYYY/MM/DD-NN/README.md`; create needed parent dirs; edit only `Semantic Version:` line in `os/BOOT.md` after entry creation. Forbidden: overwrite/modify/delete existing changelog entries; edit other source content; alter Git state.

Do:
1. Execute canonical `os/commands/SHOW_DELTAS.md` behavior; treat `SHOW_DELTA` reference as existing `SHOW_DELTAS`.
2. If no Git deltas, create nothing and leave version unchanged.
3. Use local date `YYYY/MM/DD`; inspect `change-log/YYYY/MM/DD-NN`; choose next free two-digit `NN`, starting `01`.
4. Write concise `README.md`: title `Change Log - YYYY-MM-DD NN`, Summary, Changes, Git Status.
5. Version bump in `os/BOOT.md`: major only for intentional breaking/removal of documented behavior; minor for new user-facing OS command/boot/generated artifact contract; patch for docs/clarifications/fixes/ambiguous.
6. Report created path and version transition.

Return:

```text
ADD_CHANGE_LOG complete.

Created:
- <path to README.md or "none">

Version:
- <old version> -> <new version or "unchanged">

Summary:
- <what was logged or why nothing was created>
```
