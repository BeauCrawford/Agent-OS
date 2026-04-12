# **Agent OS Command Invariants**

---

## **1. Invocation Invariants**

**COMMAND_INVOCATION_EXACT_TRIGGER_MATCHING**
Commands must activate only when user input exactly matches the defined trigger phrases. Partial matches and fuzzy interpretations are not allowed.

**COMMAND_INVOCATION_CASE_SENSITIVITY**
Trigger phrases are case-sensitive and must match the documented form precisely.

**COMMAND_INVOCATION_NO_AMBIGUITY**
Command trigger phrases must not overlap. Each trigger phrase must identify one command unambiguously.

**COMMAND_INVOCATION_CONTEXT_INDEPENDENCE**
Command invocation must succeed regardless of current workspace state, open file, or previous commands, as long as the repository or environment satisfies the command requirements.

---

## **2. Execution Invariants**

**COMMAND_EXECUTION_IDEMPOTENCY**
Running the same command multiple times in the same state must produce identical results without cumulative side effects.

**COMMAND_EXECUTION_BOUNDED_SIDE_EFFECTS**
Commands are read-only by default. A command may create, edit, or delete files only when its own command document explicitly permits that side effect and defines the allowed paths, safeguards, and cleanup scope.

**COMMAND_EXECUTION_GIT_SAFETY**
Commands must never stage, commit, reset, push, or otherwise alter Git state unless the command document explicitly permits that Git operation and active user permissions allow it.

**COMMAND_EXECUTION_RESOURCE_BOUNDS**
Execution must complete within reasonable time and memory limits, avoiding infinite loops or excessive resource consumption.

**COMMAND_EXECUTION_ERROR_HANDLING**
If prerequisites are not met, the command must fail gracefully with a clear user-facing error message and no partial execution.

**COMMAND_EXECUTION_ATOMICITY**
A command either executes fully or not at all. It must not leave intermediate states that make the system inconsistent.

**COMMAND_EXECUTION_ISOLATION**
Execution must not depend on or interfere with concurrent processes, other commands, or external tools beyond documented dependencies.

---

## **3. Output Invariants**

**COMMAND_OUTPUT_FORMAT_CONSISTENCY**
Command output must strictly adhere to the documented format, including required sections, code blocks, and structure.

**COMMAND_OUTPUT_COMPLETENESS**
All required output elements must be present.

**COMMAND_OUTPUT_ACCURACY**
Output information must be truthful and derived directly from the current state.

**COMMAND_OUTPUT_CONCISENESS**
Output must be succinct, avoiding unnecessary verbosity while preserving essential details.

**COMMAND_OUTPUT_NO_EXTRA_EXPLANATIONS**
Additional commentary or explanations must not appear outside the command's specified output structure.

**COMMAND_OUTPUT_DETERMINISTIC_CONTENT**
For the same input state, output must be identical except for explicitly allowed non-deterministic elements.

---

## **4. Behavioral Invariants**

**COMMAND_BEHAVIOR_PURPOSE_ALIGNMENT**
A command must fulfill its documented purpose exactly, without additional features or deviations.

**COMMAND_BEHAVIOR_USER_SAFETY**
Commands must prevent actions that could harm the user, workspace, repository history, system, or external accounts.

**COMMAND_BEHAVIOR_DESTRUCTIVE_SCOPE_LIMIT**
Destructive or overwriting actions must be constrained to command-owned outputs unless the user explicitly authorizes a broader scope.

**COMMAND_BEHAVIOR_PRIVACY_PRESERVATION**
Commands must not expose credentials, private data, or unrelated sensitive content in output.

**COMMAND_BEHAVIOR_ACCESSIBILITY**
Output must be readable and understandable by humans, using plain language and standard formats.

**COMMAND_BEHAVIOR_NO_EXTERNAL_DEPENDENCIES_BEYOND_SCOPE**
Commands must rely only on tools and libraries explicitly allowed by the command document.

**COMMAND_BEHAVIOR_VERSION_COMPATIBILITY**
Commands must work with standard supported versions of documented dependencies and fail gracefully when dependencies are incompatible.

---

## **5. Validation Invariants**

**COMMAND_VALIDATION_SELF_CONSISTENCY**
Output elements must not contradict each other.

**COMMAND_VALIDATION_INPUT_VALIDATION**
Commands must validate all inputs and required states before proceeding, rejecting invalid scenarios.

**COMMAND_VALIDATION_OUTPUT_VALIDATION**
Commands must internally verify that output meets format and content requirements before emitting it.

**COMMAND_VALIDATION_AUDITABILITY**
Execution should be traceable through reproducible steps or logs, though internal trace details need not be exposed to the user.

**COMMAND_VALIDATION_GLOBAL_RULE_COMPLIANCE**
Commands must adhere to overarching OS invariants, active tool permissions, repository safety rules, and command-specific side-effect scopes.

---

## **6. Example Application Invariants**

**COMMAND_EXAMPLE_SHOW_DELTAS_INVOCATION**
`SHOW_DELTAS` only triggers on `SHOW_DELTAS`.

**COMMAND_EXAMPLE_SHOW_DELTAS_EXECUTION**
`SHOW_DELTAS` runs Git status and diff commands without staging or committing.

**COMMAND_EXAMPLE_SHOW_DELTAS_OUTPUT**
`SHOW_DELTAS` provides a deltas summary and formatted commit message.

**COMMAND_EXAMPLE_SHOW_DELTAS_BEHAVIOR**
`SHOW_DELTAS` is read-only and safe for any valid Git repository state.

**COMMAND_EXAMPLE_COMPILE_INVOCATION**
`COMPILE` only triggers on `COMPILE`.

**COMMAND_EXAMPLE_COMPILE_EXECUTION**
`COMPILE` reads source Agent OS Markdown and regenerates the compiled `/os/min` Markdown set.

**COMMAND_EXAMPLE_COMPILE_OUTPUT**
`COMPILE` reports generated files, removed stale files, and the compiled boot path.

**COMMAND_EXAMPLE_COMPILE_BEHAVIOR**
`COMPILE` may create, update, and remove files only under `/os/min`; it must not alter source command documents or Git state.

---

## **Ultra-Compressed Kernel**

**Agent OS commands must execute only from exact Markdown-defined authority, remain read-only unless explicitly scoped otherwise, preserve user and workspace safety, and produce truthful output in the required command format.**
