# **COMMAND**

This command is used when the user says:

```text
COMMAND
```

The agent must explain what Agent OS commands are, how Markdown command files work, and how agents should interpret them.

---

## **Purpose**

Describe the command model for Agent OS.

Agent OS commands are Markdown-defined operating instructions that an AI agent can load, validate, and execute. A command file gives a repeatable shape to an agent task: when it is invoked, what inputs it accepts, what behavior is required, what outputs must be produced, and what constraints govern the work.

Commands are meant to make agent behavior more reliable without turning the system into a rigid program. The Markdown is the contract. The agent still uses judgment, but only inside the scope the command defines.

---

## **What a Command Is**

A command is an `.md` file that describes one invokable agent behavior.

A command may define:

- The exact trigger phrase or invocation pattern.
- The command's purpose.
- Required and optional inputs.
- Input constraints, defaults, and validation rules.
- Required behavior and decision rules.
- Allowed tools, files, directories, or external systems.
- Side-effect permissions, such as read-only, file-writing, Git operations, network calls, or publishing.
- Output constraints, including required sections, code blocks, data formats, or final response shape.
- Failure behavior for missing inputs, invalid state, denied permissions, or unavailable tools.
- Examples that show correct use.

A command should be narrow enough that an agent can execute it predictably, but complete enough that the agent does not need to infer hidden rules from the command name.

---

## **How Commands Work**

When a command is invoked, the agent must:

- Match the user's message against the documented trigger phrase or invocation pattern.
- Load the command Markdown as the source of truth for that command.
- Bind required inputs from the invocation, conversation, workspace, or documented defaults.
- Validate input constraints before taking action.
- Check required tools, files, permissions, environment state, and side-effect scope.
- Execute only the behavior allowed by the command.
- Produce output in the documented shape.
- Stop cleanly if required inputs, prerequisites, or permissions are missing.

Commands are not fuzzy suggestions. They are executable operating context for the agent.

---

## **Invocation Rules**

Commands should prefer exact triggers.

For example:

```text
SHOW_DELTAS
```

By default:

- Trigger phrases are case-sensitive.
- Partial matches do not invoke a command.
- Similar natural-language requests do not invoke a command unless the command document allows that pattern.
- A trigger must identify one command unambiguously.
- If invocation is ambiguous, the agent asks for clarification before executing.

---

## **Input Rules**

Command inputs must be explicit.

Each input should define:

- Name.
- Whether it is required or optional.
- Accepted format.
- Allowed values or range.
- Default value, if any.
- Where the value may come from.
- What to do when the value is missing or invalid.

If a required input is missing and cannot be inferred safely, the agent must ask for the missing value instead of guessing.

---

## **Output Rules**

Output constraints are part of the command contract.

A command may require:

- A specific section order.
- A fenced code block.
- A machine-readable format such as JSON, YAML, Markdown, or plain text.
- No extra commentary outside the format.
- A final summary, artifact path, commit message, patch, checklist, or validation result.

When a command defines a strict output shape, the agent must follow it. If the agent cannot produce the required output safely or truthfully, it must explain the blocker rather than returning malformed output.

---

## **Side-Effect Rules**

Commands are read-only unless they explicitly permit side effects.

A side effect includes:

- Creating, editing, moving, or deleting files.
- Formatting or rewriting code.
- Staging, committing, rebasing, resetting, or pushing Git changes.
- Installing dependencies.
- Calling external APIs.
- Publishing content.
- Sending messages or opening pull requests.
- Changing system, project, or tool configuration.

Even when a command permits side effects, active agent permissions and user approvals still apply.

---

## **Recommended Command Shape**

Use this structure when creating a new command:

````markdown
# **COMMAND_NAME**

This command is used when the user says:

```text
COMMAND_NAME
```

<One sentence describing what the agent must do.>

---

## **Purpose**

<Why this command exists.>

---

## **Input**

- `<input>`: <required or optional; format; constraints; default if any.>

---

## **Required Behavior**

When invoked, the agent must:

- <Required step or rule.>
- <Allowed side effects or read-only scope.>

---

## **Output**

<Required response format.>

---

## **Failure Behavior**

If the command cannot run, the agent must:

- <How to report missing input, invalid state, denied permission, or unavailable tools.>
````

---

## **Required Behavior**

When invoked, the agent must:

- Explain that Agent OS commands are Markdown files that define invokable agent behavior.
- Describe the command lifecycle: trigger, input binding, validation, execution, output, and failure handling.
- Emphasize that command Markdown is the source of truth.
- Emphasize that input and output constraints are binding.
- Emphasize that commands are read-only unless side effects are explicitly allowed.
- Keep the explanation concise enough to be useful as operating context for an AI agent.

---

## **Response Format**

Use this shape:

```text
Agent OS commands are Markdown-defined instructions for repeatable agent behavior.

They work by:
- <short lifecycle point>
- <short lifecycle point>
- <short lifecycle point>

A good command file defines:
- <short authoring point>
- <short authoring point>
- <short authoring point>

Important constraints:
- <short constraint point>
- <short constraint point>
- <short constraint point>
```

---

## **Failure Behavior**

If the command system cannot be described from the available local documents, the agent must say what context is missing and avoid inventing repository-specific rules.
