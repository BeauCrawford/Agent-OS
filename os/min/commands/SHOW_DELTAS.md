# SHOW_DELTAS

Trigger `SHOW_DELTAS`. Effects none. Goal: summarize current Git deltas and end with commit message.

Do: from repo root run read-only `git status --short`, `git diff --stat`, `git diff --name-status`, `git diff`, cached variants, and `git ls-files --others --exclude-standard`; include staged/unstaged/deleted/renamed/untracked; read enough untracked content to summarize substance; do not modify files/Git.

Source: canonical direct command `os/commands/SHOW_DELTAS.md`.

Return:

````text
Current deltas:

<short git status block>

Summary:
- <file>: <substantive change>

```text
<Short imperative subject>

<One concise paragraph explaining what changed and why it matters.>

<Distinctive closing sentence.>
```
````

Closing sentence: locally fitting, e.g. `Authority flows from the documents the agent loads.`
