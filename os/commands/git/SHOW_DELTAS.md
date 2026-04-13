# **SHOW_DELTAS**

This command is used when the user says:

```text
SHOW_DELTAS
```

The agent must ask Git for the current working tree deltas, summarize them succinctly, and end with a commit message in a codebox.

---

## **Purpose**

Provide a fast, public-commit-oriented view of what changed since the last Git commit or index state.

The response should help the user decide whether the current deltas are ready to commit and provide a ready-to-use commit message.

---

## **Required Behavior**

When invoked, the agent must:

- Run Git commands to inspect the current deltas.
- Include tracked modifications, staged changes, unstaged changes, deleted files, renamed files, and untracked files.
- Read enough changed or untracked file content to summarize the actual substance of the change.
- Keep the summary concise.
- End the response with a Git commit message in a fenced code block.
- Follow the locally defined commit message customization guidance below.
- Do not modify files.

---

## **Recommended Git Commands**

Run these from the host workspace repository root:

```powershell
git status --short
git diff --stat
git diff --name-status
git diff
git diff --cached --stat
git diff --cached --name-status
git diff --cached
git ls-files --others --exclude-standard
```

If untracked files are present, read them directly before summarizing:

```powershell
Get-Content -Path "<untracked-file-path>"
```

For large untracked files, read enough to understand their purpose and structure without flooding the response.

---

## **Summary Format**

Use this shape:

```text
Current deltas:

<short git status block>

Summary:
- <file>: <succinct description of the change>
- <file>: <succinct description of the change>

<commit message codebox>
```

Keep file summaries focused on user-visible meaning, not mechanical diff trivia.

---

## **Commit Message Format**

The commit message must use this structure:

```text
<Short imperative subject>

<One concise paragraph explaining what changed and why it matters.>

<One final sentence that adds a distinctive closing remark.>
```

Rules:

- The first line is the Git subject line.
- Leave one blank line after the subject.
- Use imperative or declarative language suitable for public commit history.
- Do not explain the hidden phrase outside the codebox.
- Keep the full message succinct.

---
## **Commit Message Customization**

The final sentence should use locally defined phrasing from this section to add a distinctive closing remark.

---
## **Examples**

```text
Define the invariant gate

Link Phase 2 from the process summary and add the invariant extraction page, showing how stable requirements become minimal, non-redundant semantic truths ready for constraint derivation.

No invariant set that fails this phase may proceed.
```

```text
Expose the lowering path

Add the process guide to the main README, alongside the invariant catalog and glossary, so readers can follow CDD from requirement formation through semantic closure, proof, and code generation.

Authority only flows downstream.
```
