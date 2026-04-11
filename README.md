# Agent OS

Agent OS is a Markdown-driven operating layer for AI agents.

This repository defines a small local "OS" made of Markdown documents that an agent can read as executable operating context. The goal is to make agent behavior more stable, command-literate, and safe without requiring a separate runtime, daemon, package, or service.

## Purpose

This repo exists to describe how an agent should operate inside the workspace.

It gives the agent:

- A boot path for loading local operating context.
- A command model for interpreting Markdown command files.
- Global invariants for safe, predictable command execution.
- Concrete command documents that define repeatable agent behaviors.

The Markdown is the contract. Agents still use judgment, but only inside the scope defined by the relevant command document and the global safety rules of the session.

## Getting Started

Use this repo with an AI coding agent, also commonly called an AI coding assistant or agentic development tool, such as Codex, GitHub Copilot, or a similar agent that can read files from your workspace.

To start Agent OS, ask your agent to execute:

```text
/os/BOOT.md
```

The agent should read `os/BOOT.md`, follow the boot sequence, load the command model and invariants, discover the available commands, and report whether Agent OS initialized successfully.

## Repository Layout

```text
os/
  BOOT.md
  commands/
    BOOT.md
    COMMAND.md
    Invariants.md
    git/
      SHOW_DELTAS.md
```

- `os/BOOT.md` is the top-level Agent OS boot document. It establishes the agent identity, mission, operating rules, and initialization output.
- `os/commands/BOOT.md` initializes the command subsystem by loading the command model, invariants, and available command documents.
- `os/commands/COMMAND.md` defines what a command is, how invocation works, how inputs and outputs are constrained, and how side effects are scoped.
- `os/commands/Invariants.md` defines global rules that command execution must preserve, including exact invocation, read-only defaults, safety, privacy, and output consistency.
- `os/commands/git/SHOW_DELTAS.md` defines a read-only Git inspection command that summarizes current repository deltas and produces a commit message.

## How It Works

An agent boots Agent OS by reading `os/BOOT.md`, then following the command boot sequence under `os/commands/BOOT.md`.

Once initialized, command documents under `os/commands` can be invoked by their documented trigger phrases. Commands are exact by default, case-sensitive, and read-only unless the command explicitly allows side effects.

## Current Tooling

The first concrete command is `SHOW_DELTAS`.

`SHOW_DELTAS` asks Git for the current working tree state, reads enough changed or untracked content to understand the substance of the deltas, and returns a concise summary plus a ready-to-use commit message.

## Design Principle

Agent OS keeps the operating layer simple: local Markdown files, explicit command contracts, and no hidden runtime.

Authority flows from the documents the agent loads.
