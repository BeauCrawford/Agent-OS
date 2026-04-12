# **Agent OS Glossary**

See [Agent OS Invariants](/os/Invariants.md) for the canonical invariant catalog that defines the non-negotiable operating rules behind these terms and governs how Agent OS loads, dispatches, compiles, and safely executes Markdown-defined commands.

---

## **Agent OS**

**Definition:**  
A Markdown-defined operating layer that gives an AI coding agent local identity, mission, command semantics, invariants, and side-effect boundaries inside a repository.

**Invariants:**  
AGENT_OS_IDENTITY, MARKDOWN_OPERATING_LAYER, MISSION_ALIGNMENT

**Constraints Impacted:**  
All boot, command, compilation, safety, output, and failure behavior operates under Agent OS authority after boot.

**Notes:**  
Agent OS is not a separate runtime. The documents are the system.

**Synonyms (disallowed or mapped):**  
- Agent Runtime -> AGENT_OS
- Agent Framework -> AGENT_OS

---

## **Boot**

**Definition:**  
The process by which an agent loads the active Agent OS entrypoint and establishes the operating frame for the session.

**Invariants:**  
BOOT_AUTHORITY, FULL_BOOT_SEQUENCE, MINIMAL_BOOT_SEQUENCE, INITIALIZATION_STATUS

**Constraints Impacted:**  
Boot determines which OS documents are authoritative and which command dispatch rules are active.

**Notes:**  
Full boot starts at `os/BOOT.md`; minimal boot starts at `os/min/BOOT.md`.

**Synonyms (disallowed or mapped):**  
- Initialize -> BOOT
- Startup -> BOOT

---

## **Full Boot**

**Definition:**  
The source boot path that loads `os/BOOT.md`, OS invariants, command boot context, command model, command invariants, and command discovery.

**Invariants:**  
FULL_BOOT_SEQUENCE, MISSING_BOOT_CONTEXT, IDEMPOTENT_BOOT

**Constraints Impacted:**  
Defines the complete source operating frame and command subsystem initialization.

**Notes:**  
Full boot is richer than min boot and is intended for source-authoritative operation.

**Synonyms (disallowed or mapped):**  
- Source Boot -> FULL_BOOT

---

## **Minimal Boot**

**Definition:**  
The compiled boot path that loads only `os/min/BOOT.md` and defers command-specific files until exact trigger match.

**Invariants:**  
MINIMAL_BOOT_SEQUENCE, LAZY_COMMAND_LOADING, MINIMAL_BOOT_LOAD

**Constraints Impacted:**  
Defines low-token command routing and cache behavior for LLM-efficient operation.

**Notes:**  
Minimal boot is produced by `COMPILE` and optimized for LLM context economy.

**Synonyms (disallowed or mapped):**  
- Min Boot -> MINIMAL_BOOT
- Compiled Boot -> MINIMAL_BOOT

---

## **Command**

**Definition:**  
A Markdown file that defines an exact invokable agent behavior, including trigger, purpose, required context, allowed side effects, output shape, and failure behavior.

**Invariants:**  
SOURCE_OF_TRUTH, EXACT_DISPATCH, CASE_SENSITIVITY, FORMAT_FIDELITY

**Constraints Impacted:**  
All command execution must follow the loaded command document.

**Notes:**  
Commands are contracts, not suggestions.

**Synonyms (disallowed or mapped):**  
- Action -> COMMAND
- Skill -> COMMAND

---

## **Command Registry**

**Definition:**  
A boot-time mapping from exact command triggers to the command files that should be loaded after a trigger match.

**Invariants:**  
UNAMBIGUOUS_ROUTING, LAZY_COMMAND_LOADING, TRUTHFULNESS

**Constraints Impacted:**  
Determines command routing and which files must remain unloaded until invoked.

**Notes:**  
The min OS registry lives in `os/min/BOOT.md`.

**Synonyms (disallowed or mapped):**  
- Dispatch Table -> COMMAND_REGISTRY
- Command Map -> COMMAND_REGISTRY

---

## **Invariant**

**Definition:**  
A stable operating truth that must hold across Agent OS behavior.

**Invariants:**  
SOURCE_OF_TRUTH, FORMAT_FIDELITY, SEMANTIC_PRESERVATION

**Constraints Impacted:**  
All OS and command behavior must preserve relevant invariants.

**Notes:**  
Agent OS invariants use self-documenting universal IDs such as `AGENT_OS_IDENTITY`.

**Synonyms (disallowed or mapped):**  
- Rule -> INVARIANT
- Principle -> INVARIANT

---

## **Document Authority**

**Definition:**  
The condition where loaded Agent OS Markdown governs agent behavior for the current operation.

**Invariants:**  
BOOT_AUTHORITY, LOADED_CONTEXT_AUTHORITY, SOURCE_OF_TRUTH, NO_HIDDEN_RULES

**Constraints Impacted:**  
Prevents behavior from being inferred from unloaded files or undocumented assumptions.

**Notes:**  
Authority flows from the documents the agent loads.

**Synonyms (disallowed or mapped):**  
- Source Authority -> DOCUMENT_AUTHORITY

---

## **Exact Trigger**

**Definition:**  
The exact case-sensitive phrase that invokes a command.

**Invariants:**  
EXACT_DISPATCH, CASE_SENSITIVITY, NO_FUZZY_INVOCATION

**Constraints Impacted:**  
Determines whether a command may execute.

**Notes:**  
Similar natural-language requests do not invoke commands unless the command explicitly permits that pattern.

**Synonyms (disallowed or mapped):**  
- Command Name -> EXACT_TRIGGER
- Invocation Phrase -> EXACT_TRIGGER

---

## **Lazy Loading**

**Definition:**  
The practice of deferring command-file loading until the user provides an exact command trigger.

**Invariants:**  
LAZY_COMMAND_LOADING, MINIMUM_SUFFICIENT_CONTEXT, CACHE_SCOPE

**Constraints Impacted:**  
Controls context size, command dispatch, and min OS behavior.

**Notes:**  
Lazy loading is central to the compiled `os/min` design.

**Synonyms (disallowed or mapped):**  
- Deferred Loading -> LAZY_LOADING

---

## **Side Effect**

**Definition:**  
Any action that creates, modifies, deletes, stages, commits, pushes, publishes, configures, or otherwise changes repository, system, or external state.

**Invariants:**  
READ_ONLY_DEFAULT, BOUNDED_SIDE_EFFECTS, WORKSPACE_PRESERVATION, GIT_SAFETY

**Constraints Impacted:**  
All commands are read-only unless side effects are explicitly permitted and scoped.

**Notes:**  
Permitted side effects must name their paths and operations.

**Synonyms (disallowed or mapped):**  
- Mutation -> SIDE_EFFECT
- Write -> SIDE_EFFECT

---

## **Bounded Side Effect**

**Definition:**  
A permitted side effect constrained to explicit paths, operations, and safeguards in a loaded command document.

**Invariants:**  
BOUNDED_SIDE_EFFECTS, GENERATED_OUTPUT_OWNERSHIP, PERMISSION_RESPECT

**Constraints Impacted:**  
Allows useful automation while preserving user work and active permissions.

**Notes:**  
`COMPILE` may write under `os/min`; `ADD_CHANGE_LOG` may create a new changelog entry and update the semantic version line.

**Synonyms (disallowed or mapped):**  
- Scoped Mutation -> BOUNDED_SIDE_EFFECT

---

## **COMPILE**

**Definition:**  
The command that regenerates the minimal Agent OS under `os/min` for LLM-efficient boot and lazy command loading.

**Invariants:**  
COMPILE_TARGET, LLM_OPTIMIZATION_TARGET, MINIMAL_BOOT_LOAD, STALE_OUTPUT_REMOVAL

**Constraints Impacted:**  
Defines compiled boot, generated min commands, stale output cleanup, and token-economy behavior.

**Notes:**  
`COMPILE` discovers direct child command documents from `os/commands/*.md` and excludes `Invariants.md`.

**Synonyms (disallowed or mapped):**  
- Build Min OS -> COMPILE
- Recompile -> COMPILE

---

## **Compiled OS**

**Definition:**  
The generated minimal Agent OS stored under `os/min`.

**Invariants:**  
COMPILE_TARGET, LLM_OPTIMIZATION_TARGET, SEMANTIC_PRESERVATION

**Constraints Impacted:**  
Controls low-token boot and lazy command behavior.

**Notes:**  
The compiled OS should preserve source semantics while reducing LLM context cost.

**Synonyms (disallowed or mapped):**  
- Min OS -> COMPILED_OS
- Generated OS -> COMPILED_OS

---

## **SHOW_DELTAS**

**Definition:**  
The command that inspects Git state, summarizes current repository deltas, and produces a commit message.

**Invariants:**  
EXACT_DISPATCH, READ_ONLY_DEFAULT, TRUTHFULNESS, FORMAT_FIDELITY

**Constraints Impacted:**  
Provides the source delta summary used by changelog generation.

**Notes:**  
The canonical command path is `os/commands/SHOW_DELTAS.md`.

**Synonyms (disallowed or mapped):**  
- SHOW_DELTA -> SHOW_DELTAS

---

## **ADD_CHANGE_LOG**

**Definition:**  
The command that uses `SHOW_DELTAS` output to create a dated user-friendly changelog entry and bump the semantic version when deltas exist.

**Invariants:**  
BOUNDED_SIDE_EFFECTS, GENERATED_OUTPUT_OWNERSHIP, TRUTHFULNESS, FORMAT_FIDELITY

**Constraints Impacted:**  
Controls changelog creation and semantic version updates.

**Notes:**  
Entries are written under `change-log/YYYY/MM/DD-NN/README.md`.

**Synonyms (disallowed or mapped):**  
- Changelog Entry -> ADD_CHANGE_LOG

---

## **Semantic Version**

**Definition:**  
The version value in `os/BOOT.md` that marks compatibility impact for the Agent OS operating contract.

**Invariants:**  
TRUTHFULNESS, FORMAT_FIDELITY, BOUNDED_SIDE_EFFECTS

**Constraints Impacted:**  
Updated by `ADD_CHANGE_LOG` when a changelog entry is created.

**Notes:**  
Major indicates breaking documented behavior, minor indicates new compatible user-facing capability, and patch indicates compatible fixes or clarifications.

**Synonyms (disallowed or mapped):**  
- Version -> SEMANTIC_VERSION

---

## **Changelog Entry**

**Definition:**  
A dated `README.md` file that summarizes a set of repository deltas in user-friendly language.

**Invariants:**  
TRUTHFULNESS, CONCISENESS, GENERATED_OUTPUT_OWNERSHIP

**Constraints Impacted:**  
Captures Agent OS evolution and provides a human-readable record of changes.

**Notes:**  
The directory counter increments per local date.

**Synonyms (disallowed or mapped):**  
- Change Note -> CHANGELOG_ENTRY

---

## **Cache**

**Definition:**  
Working-memory reuse of loaded OS or command context during the current agent session.

**Invariants:**  
CACHE_SCOPE, CACHE_INVALIDATION, LOADED_CONTEXT_AUTHORITY

**Constraints Impacted:**  
Controls when loaded context can be reused and when it must be invalidated.

**Notes:**  
Cache is an optimization, not a replacement for document authority.

**Synonyms (disallowed or mapped):**  
- Memory -> CACHE

---

## **Failure Blocker**

**Definition:**  
A concrete condition that prevents an Agent OS operation from safely or truthfully completing.

**Invariants:**  
FAIL_CLOSED, CLEAR_BLOCKERS, NO_PARTIAL_CLAIMS, NO_INVENTED_RECOVERY

**Constraints Impacted:**  
Determines when the agent must stop instead of guessing or partially executing.

**Notes:**  
Examples include missing required files, invalid inputs, denied permissions, and unsafe side effects.

**Synonyms (disallowed or mapped):**  
- Error -> FAILURE_BLOCKER
- Blocked State -> FAILURE_BLOCKER

---
