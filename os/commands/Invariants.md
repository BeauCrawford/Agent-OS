# Command Invariants

This document defines the exhaustive invariants that must hold true for any COMMAND in the Agent-OS system, exemplified by the `SHOW_DELTAS` command. Invariants ensure consistent, reliable, and safe command execution across all scenarios.

## Invocation Invariants

1. **Exact Trigger Matching**: The command must only activate when the user input exactly matches the defined trigger phrases (e.g., "SHOW_DELTAS" for the SHOW_DELTAS command). No partial matches or fuzzy interpretations are allowed.

2. **Case Sensitivity**: Trigger phrases are case-sensitive and must match the documented form precisely.

3. **No Ambiguity**: Commands must not overlap in trigger phrases; each phrase uniquely identifies one command.

4. **Context Independence**: Invocation must succeed regardless of the current workspace state, file open, or previous commands, as long as the repository or environment supports the command's requirements.

5. **Path Authority Separation**: Command documents may be loaded from an Agent OS repository while the user's active context window belongs to another repository. Agent OS resource paths in command documents must resolve from the loaded file or Agent OS resource root, while target workspace paths, Git state, and side effects remain relative to the host workspace unless the user or loaded command explicitly selects another target.

## Execution Invariants

6. **Idempotency**: Running the command multiple times in the same state must produce identical results, without cumulative side effects.

7. **Bounded Side Effects**: Commands are read-only by default. A command may create, edit, or delete files only when its own command document explicitly permits that side effect and defines the allowed paths, safeguards, and cleanup scope. Commands must never stage, commit, reset, push, or otherwise alter Git state unless the command document explicitly permits that Git operation and active user permissions allow it.

8. **Resource Bounds**: Execution must complete within reasonable time and memory limits (e.g., <30 seconds, <100MB memory), avoiding infinite loops or excessive resource consumption.

9. **Error Handling**: If prerequisites are not met (e.g., not in a Git repository for SHOW_DELTAS), the command must fail gracefully with a clear, user-friendly error message and no partial execution.

10. **Atomicity**: The command either executes fully or not at all; no intermediate states that leave the system inconsistent.

11. **Isolation**: Execution must not depend on or interfere with concurrent processes, other commands, or external tools beyond its documented dependencies.

## Output Invariants

12. **Format Consistency**: Output must strictly adhere to the documented format, including sections, code blocks, and structure (e.g., deltas summary followed by commit message codebox).

13. **Completeness**: All required elements must be present (e.g., for SHOW_DELTAS: Git status, file summaries, and commit message).

14. **Accuracy**: Information must be truthful and derived directly from the current state (e.g., Git diffs reflect actual changes).

15. **Conciseness**: Output must be succinct, avoiding unnecessary verbosity while including all essential details.

16. **No Explanations Outside Format**: Additional commentary or explanations must not appear outside the specified output structure.

17. **Deterministic Content**: For the same input state, output must be identical, barring non-deterministic elements like timestamps (which are not present in SHOW_DELTAS).

## Behavioral Invariants

18. **Purpose Alignment**: The command must fulfill its documented purpose exactly, without additional features or deviations.

19. **User Safety**: Must prevent any actions that could harm the user, workspace, or system. Destructive or overwriting actions must be constrained to command-owned outputs unless the user explicitly authorizes a broader scope.

20. **Privacy Preservation**: Must not expose sensitive information (e.g., credentials, private data) in output.

21. **Accessibility**: Output must be readable and understandable by humans, using plain language and standard formats.

22. **No External Dependencies Beyond Scope**: Must only rely on tools and libraries explicitly allowed for the command (e.g., Git for SHOW_DELTAS).

23. **Version Compatibility**: Must work with standard versions of dependencies (e.g., Git 2.x+), failing gracefully if incompatible.

## Validation Invariants

24. **Self-Consistency**: Output elements must not contradict each other (e.g., summary matches the detailed diffs).

25. **Input Validation**: Must validate all inputs and states before proceeding, rejecting invalid scenarios.

26. **Output Validation**: The command must internally verify that output meets format and content requirements before emitting.

27. **Auditability**: Execution should be traceable through logs or reproducible steps, though not exposed to the user.

28. **Compliance with Global Rules**: Must adhere to overarching system invariants, active tool permissions, repository safety rules, and command-specific side-effect scopes.

## Example Application to SHOW_DELTAS

- **Invocation**: Only triggers on "SHOW_DELTAS".
- **Execution**: Runs Git status/diff commands without staging or committing.
- **Output**: Provides deltas summary and formatted commit message.
- **Behavior**: Read-only, safe for any Git repository state.

## Example Application to COMPILE

- **Invocation**: Only triggers on "COMPILE".
- **Execution**: Reads source Agent OS Markdown and regenerates the compiled `/os/min` Markdown set.
- **Output**: Reports generated files, removed stale files, and boot path.
- **Behavior**: May create, update, and remove files only under `/os/min`; it must not alter source command documents or Git state.

These invariants ensure commands like SHOW_DELTAS are reliable, safe, and predictable.
