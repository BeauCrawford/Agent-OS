# **BOOT**

This command is used when the user says:

```text
BOOT
```

The agent must initialize Agent OS by loading the command model, the command invariants, and the available command documents into working context.

---

## **Purpose**

Initialize Agent OS for the current agent session.

BOOT establishes the operating frame for Markdown-driven commands. It begins by loading what a command is, then loads the invariants that govern all commands, then discovers the available command files that can be invoked during the session.

---

## **Required Context**

The agent must load these Markdown files first:

1. [COMMAND](./COMMAND.md)
2. [Command Invariants](./Invariants.md)

The `COMMAND.md` file defines the concept of a command: how command documents work, how invocation is matched, how inputs are bound, how outputs are constrained, and how side effects are scoped.

The `Invariants.md` file defines the global command contract that applies to command invocation, input validation, execution, output, safety, privacy, and failure behavior.

---

## **Command Discovery**

After loading the required context, the agent must discover other command Markdown files under this directory tree:

```text
os/commands/**/*.md
```

The agent must treat each discovered `.md` file as a possible command document, except that `Invariants.md` is supporting command context rather than an invokable user command unless it explicitly defines an invocation.

---

## **Required Behavior**

When invoked, the agent must:

- Read this `BOOT.md` command file.
- Read [COMMAND](./COMMAND.md) into context.
- Read [Command Invariants](./Invariants.md) into context.
- Discover available command Markdown files under `os/commands`.
- Report that Agent OS is initialized.
- List the command documents loaded or discovered.
- Keep the operation read-only.

---

## **Output**

Use this shape:

```text
Agent OS initialized.

Loaded core context:
- os/commands/COMMAND.md
- os/commands/Invariants.md

Discovered commands:
- <command path>
- <command path>

Notes:
- <short note about command invocation, constraints, or any missing context>
```

---

## **Failure Behavior**

If required files are missing, the agent must:

- Stop initialization.
- Report which required file is missing.
- Avoid treating the OS as initialized.

If command discovery fails, the agent must:

- Report that core context was loaded.
- Report that command discovery failed.
- Avoid inventing command files.
