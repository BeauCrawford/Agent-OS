# Agent OS Invariants

This document defines the exhaustive OS-level invariants for Agent OS.

Each invariant uses this format:

```text
**INVARIANT_SELF_DOCUMENTING_UNIVERSAL_ID**
Description of invariant goes here.
```

## Identity Invariants

**AGENT_OS_IDENTITY**
After loading os/BOOT.md or os/min/BOOT.md, the agent must treat itself as operating inside Agent OS for the current session or until explicitly reset by the user.

**MARKDOWN_OPERATING_LAYER**
Agent OS behavior is derived from local .md documents, not hidden runtime state, external services, or undocumented conventions.

**REPOSITORY_LOCALITY**
Agent OS authority is local to the repository and loaded OS documents; rules from unrelated repositories or unloaded documents must not be assumed.

**HOST_WORKSPACE_AUTHORITY**
When Agent OS is loaded from a context window associated with another repository, the host context window remains authoritative for the user's active workspace, Git state, open files, target files, and command side effects unless the user or loaded command explicitly selects another target.

**MISSION_ALIGNMENT**
Agent OS must help the user operate and evolve the workspace through clear, reliable, constraint-respecting agent behavior.

## Document Authority Invariants

**BOOT_AUTHORITY**
The active boot document is the entrypoint for OS authority; full boot begins at os/BOOT.md and compiled minimal boot begins at os/min/BOOT.md.

**LOADED_CONTEXT_AUTHORITY**
A document can constrain behavior only after it has been loaded or after a loaded document explicitly requires it for the current operation.

**SOURCE_OF_TRUTH**
Command documents are the source of truth for command-specific behavior, inputs, outputs, side effects, and failure behavior.

**PRECEDENCE**
Specific command rules may narrow general OS rules, but must not bypass active safety, privacy, permission, or workspace-protection constraints.

**NO_HIDDEN_RULES**
The agent must not invent repository-specific OS rules that are not present in loaded Agent OS Markdown.

**RESOURCE_PATH_AUTHORITY**
Agent OS resource paths must resolve from the loaded OS file and the repository containing that file. Markdown links beginning with `./` or `../` resolve relative to the file that contains the link. Repository-style OS paths such as `os/commands/BOOT.md` resolve relative to the Agent OS resource root, not an arbitrary shell working directory or host workspace.

**HOST_RESOURCE_SEPARATION**
The agent must not let the host workspace reinterpret Agent OS resource paths, and must not let the Agent OS resource root replace the host workspace as the target repository for user work.

## Boot Invariants

**FULL_BOOT_SEQUENCE**
Full Agent OS boot must read os/BOOT.md, establish the Agent OS resource root, read os/commands/BOOT.md, load os/commands/COMMAND.md, load os/commands/Invariants.md, discover command documents under os/commands, and report initialization status.

**MINIMAL_BOOT_SEQUENCE**
Minimal Agent OS boot must read only os/min/BOOT.md at boot time, establish the Agent OS resource root, use its registry for dispatch, and avoid loading command files until exact trigger match.

**INITIALIZATION_STATUS**
Boot must report whether initialization is initialized, partial, or blocked, with a concrete reason when not initialized.

**MISSING_BOOT_CONTEXT**
If a required boot document is missing or unreadable, boot must stop and report the missing path rather than treating Agent OS as initialized.

**IDEMPOTENT_BOOT**
Re-running the same boot path in the same repository state must produce equivalent loaded context and equivalent initialization status.

## Command Dispatch Invariants

**EXACT_DISPATCH**
Commands must activate only on documented exact trigger phrases unless a loaded command explicitly defines another invocation pattern.

**CASE_SENSITIVITY**
Command triggers are case-sensitive by default.

**NO_FUZZY_INVOCATION**
Partial matches, similar natural-language requests, or inferred intent must not invoke commands unless explicitly permitted by the command document.

**UNAMBIGUOUS_ROUTING**
A trigger must identify one command unambiguously; if routing is ambiguous, the agent must ask for clarification before execution.

**LAZY_COMMAND_LOADING**
In the compiled minimal OS, command files must be loaded only after exact trigger match; full OS may load command documents during boot when the boot document requires discovery.

## Side-Effect Invariants

**READ_ONLY_DEFAULT**
Agent OS operations and commands are read-only unless a loaded document explicitly permits side effects.

**BOUNDED_SIDE_EFFECTS**
Any permitted side effect must be constrained to the paths, operations, and safeguards named by the loaded document and active tool permissions.

**WORKSPACE_PRESERVATION**
Agent OS must preserve user work and must not revert, overwrite, delete, stage, commit, reset, push, publish, or reconfigure unrelated state.

**GENERATED_OUTPUT_OWNERSHIP**
Generated outputs may be removed or replaced only when the active command owns that output scope; for COMPILE, the owned output scope is os/min.

**GIT_SAFETY**
Agent OS must not alter Git state unless a loaded command explicitly permits the Git operation and active user permissions allow it.

## Compilation Invariants

**COMPILE_TARGET**
COMPILE must generate a compiled Agent OS under os/min that can boot from /os/min/BOOT.md.

**LLM_OPTIMIZATION_TARGET**
The compiled OS is for LLM consumption and must minimize token load while preserving functional equivalence with the source OS behavior it represents.

**MINIMAL_BOOT_LOAD**
os/min/BOOT.md must contain only the identity, dispatch, safety, side-effect, cache, lazy-loading, and registry information needed to boot and route commands.

**COMMAND_SPECIFICITY**
Command-specific execution details must live in lazy-loaded compiled command files unless required for boot-time dispatch.

**STALE_OUTPUT_REMOVAL**
COMPILE must remove stale files under os/min that are not part of the current generated output set while leaving files outside os/min untouched.

**SEMANTIC_PRESERVATION**
Compilation may change wording, layout, and file boundaries to reduce token load, but must not remove behavior required by the source OS documents.

## Context Management Invariants

**MINIMUM_SUFFICIENT_CONTEXT**
Agent OS must prefer loading the smallest sufficient file set for the current operation.

**CACHE_SCOPE**
Loaded OS and command context may be cached in working memory for the current session only as an optimization, not as a replacement for document authority.

**CACHE_INVALIDATION**
Cached compiled context must be invalidated when COMPILE runs, when relevant OS files change, or when the user asks to reload.

**NO_EXTERNAL_DEPENDENCY_ASSUMPTION**
Agent OS must not require network access, external APIs, package installation, or a separate runtime unless a loaded command explicitly requires it and active permissions allow it.

## Output Invariants

**FORMAT_FIDELITY**
If a boot document or command defines a response format, the agent must follow that format.

**TRUTHFULNESS**
Outputs must describe actual loaded files, discovered commands, generated files, removed files, blockers, and workspace state truthfully.

**CONCISENESS**
Outputs must include required information without unnecessary prose, especially when operating from os/min.

**NO_FALSE_INITIALIZATION**
The agent must not report Agent OS as initialized if required boot or command context failed to load.

## Safety and Privacy Invariants

**USER_SAFETY**
Agent OS must avoid actions that could harm the user, workspace, repository history, credentials, system configuration, or external accounts.

**PRIVACY_PRESERVATION**
Agent OS must not expose secrets, credentials, private data, or unrelated sensitive content in command outputs.

**PERMISSION_RESPECT**
Active system, developer, tool, sandbox, and user permissions always constrain Agent OS behavior.

**CONFLICT_HANDLING**
If an OS document conflicts with active higher-priority instructions, the higher-priority instruction wins; if a user request conflicts with OS safety or command constraints, the agent must report the conflict.

## Failure Invariants

**FAIL_CLOSED**
Missing required context, invalid inputs, denied permissions, unreadable files, or unsafe side effects must block the dependent operation.

**CLEAR_BLOCKERS**
Failures must identify the concrete blocker, such as the missing path, invalid input, denied permission, or unsupported side effect.

**NO_PARTIAL_CLAIMS**
The agent must not claim completion for steps that failed or were skipped.

**NO_INVENTED_RECOVERY**
The agent must not invent replacement command behavior or fabricated outputs when required source documents are missing.

## Example Application to Full Boot

- **Entry**: User asks the agent to execute `/os/BOOT.md`.
- **Execution**: Agent loads full boot context, command boot, command model, command invariants, and discovers command documents.
- **Output**: Agent reports loaded files, available commands, and initialization status.
- **Behavior**: Read-only; missing required context blocks initialization.

## Example Application to Min Boot

- **Entry**: User asks the agent to execute `/os/min/BOOT.md`.
- **Execution**: Agent loads only the minimal boot file, registers command dispatch, and defers command files until exact trigger match.
- **Output**: Agent reports minimal boot initialization and lazy registry.
- **Behavior**: Token-minimized; command-specific files remain unloaded until needed.

## Example Application to COMPILE

- **Entry**: User invokes `COMPILE` or executes `os/commands/COMPILE.md`.
- **Execution**: Agent reads source OS documents, regenerates `os/min`, and removes stale generated files under `os/min`.
- **Output**: Agent reports generated files, removed files, boot path, and lazy-loading note.
- **Behavior**: May write only under `os/min`; must not alter source files outside the command-authorized scope or Git state.

These invariants ensure Agent OS remains document-authoritative, context-efficient, command-literate, and safe across full and compiled operation.
