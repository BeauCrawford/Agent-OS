# Theory of Agent OS

Agent OS is a Markdown-defined operating layer for AI coding agents.

The core idea is simple: an agent can behave more reliably when the workspace contains explicit local documents that define identity, mission, command semantics, invariants, side effects, and output formats. Agent OS treats those Markdown files as operating context rather than passive documentation.

## Markdown as Operating Context

Most repositories use Markdown to explain work to humans. Agent OS also uses Markdown to shape agent behavior.

An Agent OS document is written so an agent can load it, interpret it, and act within its constraints. The document does not need a runtime or service. Its authority comes from being local, readable, explicit, and referenced by the active boot path.

This keeps the system lightweight:

- The repository carries its own operating rules.
- The user can inspect and edit those rules directly.
- The agent does not need hidden configuration to understand how to work.
- Behavior can evolve through ordinary file changes.

## Boot Creates the Operating Frame

Boot is the moment an agent enters Agent OS.

The full boot path starts at `os/BOOT.md`. It establishes the Agent OS identity, loads OS-level invariants, loads the command subsystem, and discovers available command documents.

The compiled boot path starts at `os/min/BOOT.md`. It is optimized for LLM context economy: it loads only the minimal dispatch rules and registry, then defers command-specific files until an exact command trigger is used.

Both boot paths exist for the same reason: they make the operating frame explicit.

## Commands Are Contracts

An Agent OS command is a Markdown file that defines an invokable behavior.

A good command answers:

- What exact trigger invokes it?
- What inputs are required?
- What context must be loaded?
- What side effects are allowed?
- What output shape must be returned?
- What happens when prerequisites are missing?

This turns agent behavior from an informal request into a repeatable contract. The agent still uses judgment, but that judgment is bounded by the loaded command document.

## Invariants Define What Must Not Drift

Invariants are the stable truths of the system.

Agent OS uses invariants to make clear which properties must hold across boot, dispatch, command execution, compilation, safety, privacy, output, and failure behavior.

The invariant format is intentionally self-documenting:

```markdown
**AGENT_OS_IDENTITY**
After loading os/BOOT.md or os/min/BOOT.md, the agent must treat itself as operating inside Agent OS for the current session or until explicitly reset by the user.
```

This makes each invariant easy to reference by name while keeping the rule readable.

## Side Effects Are Explicit

Agent OS is read-only by default.

A command may create, modify, or remove files only when the command document explicitly permits that side effect and names the allowed scope. For example, `COMPILE` can regenerate files under `os/min`, and `ADD_CHANGE_LOG` can create a new changelog entry and update the semantic version line in `os/BOOT.md`.

This model protects user work while still allowing useful automation.

## Compilation Is Context Engineering

`COMPILE` builds a minimal Agent OS under `os/min`.

The compiled OS is for LLM consumption. Its goal is functional equivalence at lower token cost. It keeps boot small, stores command-specific behavior in lazy-loaded command files, and removes redundant prose that is useful for humans but unnecessary during operation.

In other words, the source OS explains the system richly. The min OS executes it efficiently.

## Changelog as Memory

`ADD_CHANGE_LOG` converts current deltas into dated user-facing changelog entries.

This gives Agent OS a lightweight memory of how it evolves. The changelog records what changed and why it matters, while the semantic version in `os/BOOT.md` marks the compatibility level of the operating contract.

The result is a loop:

1. Change the OS documents.
2. Compile when needed.
3. Capture the change in the changelog.
4. Bump the semantic version according to impact.

## Design Principle

Agent OS is not a framework wrapped around an agent. It is a local contract the agent agrees to load and obey.

The documents are the system.
