# **ADD_CHANGE_LOG**

This command is used when the user says:

```text
ADD_CHANGE_LOG
```

The agent must run the existing `SHOW_DELTAS` command, use its output to create a new user-friendly changelog entry under `change-log/YYYY/MM/DD-NN/README.md`, and increment the semantic version in `os/BOOT.md` accordingly.

---

## **Purpose**

Capture the current repository deltas as a dated, human-readable changelog entry.

The changelog is meant for users and collaborators who want to understand what changed and why without reading raw Git diffs.

---

## **Required Context**

The agent must load and execute:

```text
os/commands/git/SHOW_DELTAS.md
```

`SHOW_DELTAS` is the source of truth for detecting and summarizing current Git deltas. If the user or another document refers to `SHOW_DELTA`, treat it as a reference to the existing `SHOW_DELTAS` command unless a separate `SHOW_DELTA` command document is later added.

---

## **Side Effects**

This command may:

- Create the `change-log` directory if missing.
- Create date directories under `change-log/YYYY/MM`.
- Create exactly one new entry directory named `change-log/YYYY/MM/DD-NN`, where `NN` is the next two-digit incrementing counter for that date.
- Create `README.md` inside that new entry directory.
- Update the `Semantic Version: ` line in `os/BOOT.md` when and only when a changelog entry is created.

This command must not:

- Modify existing changelog entries.
- Overwrite an existing `README.md`.
- Delete changelog entries.
- Modify source files outside the new changelog entry except the `Semantic Version: ` line in `os/BOOT.md`.
- Stage, commit, reset, or otherwise alter Git state.

---

## **Counter Rules**

When creating the entry directory:

1. Use the current local date in `YYYY/MM/DD` form.
2. Inspect existing directories under `change-log/YYYY/MM` that match `DD-NN`.
3. Choose the next available two-digit counter for that date.
4. Start with `01` when no entry exists for the date.
5. If the chosen path already exists, increment until a free path is found.

Examples:

```text
change-log/2026/04/11-01/README.md
change-log/2026/04/11-02/README.md
```

---

## **Semantic Version Rules**

When a changelog entry is created, increment the semantic version in `os/BOOT.md`.

The version line must match this shape:

```text
Semantic Version: `MAJOR.MINOR.PATCH`
```

Choose the increment from the `SHOW_DELTAS` summary:

1. Increment `MAJOR` and reset `MINOR` and `PATCH` to `0` only when the deltas intentionally remove or break existing documented OS or command behavior.
2. Increment `MINOR` and reset `PATCH` to `0` when the deltas add a new user-facing OS capability, command, boot behavior, or generated artifact contract.
3. Increment `PATCH` for documentation, clarification, invariant tightening, changelog-only updates, bug fixes, or small compatible behavior changes.
4. If the correct increment is ambiguous, use `PATCH`.
5. If no changelog entry is created because no deltas exist, do not change the semantic version.

Only update the semantic version line. Do not rewrite unrelated `os/BOOT.md` content.

---

## **Required Behavior**

When invoked, the agent must:

1. Run `SHOW_DELTAS` exactly as defined by `os/commands/git/SHOW_DELTAS.md`.
2. Determine whether any deltas exist from the Git status and diff data.
3. If no deltas exist, do not create a changelog entry and do not update the semantic version.
4. If deltas exist, create the next dated entry directory.
5. Create `README.md` in that directory.
6. Write a user-friendly changelog summary based on the `SHOW_DELTAS` output.
7. Include the Git status block or a concise changed-file list.
8. Focus on what changed and why it matters, not mechanical diff details.
9. Increment the semantic version in `os/BOOT.md` according to the semantic version rules.
10. Report the created changelog path and new semantic version.

---

## **Changelog Entry Format**

Use this shape for the generated `README.md`:

````markdown
# Change Log - YYYY-MM-DD NN

## Summary

<Short user-friendly paragraph describing the change set.>

## Changes

- <file or area>: <user-friendly change summary>
- <file or area>: <user-friendly change summary>

## Git Status

```text
<short git status block from SHOW_DELTAS>
```
````

Keep the entry concise. Do not include full raw diffs unless the user explicitly asks for them.

---

## **Output**

Use this shape:

```text
ADD_CHANGE_LOG complete.

Created:
- <path to README.md or "none">

Version:
- <old version> -> <new version or "unchanged">

Summary:
- <short summary of what was logged or why nothing was created>
```

---

## **Failure Behavior**

If the command cannot run, the agent must:

- Report the missing command document, Git inspection failure, invalid date path, existing target path, denied permission, or write failure.
- Avoid creating partial changelog entries when possible.
- Avoid updating `os/BOOT.md` if the changelog entry cannot be created.
- Avoid modifying existing changelog entries.
- Avoid altering Git state.
