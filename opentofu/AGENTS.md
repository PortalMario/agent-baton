# AGENTS.md — Agent Instructions

Follow exactly; all rule changes must be reviewed extra carefully, as they directly shape the code the agent produces.

## Core Principles

- **KISS:** Prefer simpler solutions that solve the problem. Avoid over-engineering, premature abstraction, and unnecessary complexity.
- **Comment sensibly:** Add meaningful comments where intent is not obvious. Explain the *why*, not the trivial *what*.
- **Comment deviations from best practices:** When intentionally deviating from a best practice, add a dedicated separate comment line explaining the reason, if possible.
- **English only:** Write all content — code, comments, identifiers, commit messages, and documentation — in English.

## OpenTofu

- Use `locals` blocks whenever resource/data source blocks would otherwise become hard to read, or when many OpenTofu built-in functions are used — factor that logic into `locals` for clarity.
- Every `locals` block MUST be documented with a one-line comment directly above it explaining its purpose.
- Always avoid executing external scripts (e.g. `local-exec`/`remote-exec` provisioners, `external` data source) — prefer native OpenTofu resources and providers.
- Every input variable MUST be explicitly declared in `variables.tf` — never introduce variables implicitly.
- Resource/data source blocks MUST always be written to be iterable (usable with `for_each`) so multiple resources can be created/managed from a single block. Design a user-friendly input variable structure/definition for this iteration.
- Whether the variable values are read in from YAML (as input vars) MUST be decided by asking the user — never assume the YAML approach on your own.
- Modules MUST be portable: no hardcoded environment-specific values (accounts, regions, IDs) — expose them as variables with sensible defaults.
- Keep modules as small and focused as possible: a module should ideally manage a single resource type and never mix unrelated resource types. For example, a module that creates an AKS cluster must not also contain Azure DNS zone resources in the same file — combining different resource types is only acceptable in absolute exceptions (document the reason).
- Stick to standard OpenTofu file names as much as possible (e.g. `main.tf`, `variables.tf`, `outputs.tf`, `providers.tf`, `versions.tf`).
- State should ideally always live in a remote, encrypted backend — never rely on an unencrypted local state for anything shared or production.
- Manual, AI-assisted, or automated editing of the OpenTofu state MUST be prevented at all costs — never hand-edit the state; change infrastructure only through configuration and `plan`/`apply`.

## README Maintenance
- README must contain at least:
  - **Description:** A short summary of what the project/component does.
  - **Usage:** How to run or use it.
  - **Parameters:** A table of invocation/call parameters when it makes sense (e.g. for CLI tools, scripts, or configurable modules).
  - **Contribution:** How to contribute to the project.
- When building an OpenTofu module, its README MUST document what the module does and list all inputs (input variables) and all outputs.


## Version Control
- A `.gitignore` tailored to the specific project MUST always exist. Populate it with the ignore patterns relevant to the project's language, tooling, and build artifacts.
- Always push to feature branches whenever possible; never push directly to the main branch.


## Security
- Never commit secrets, credentials, tokens, or private keys.
- Provider authentication data (credentials, tokens, keys) MUST never be stored in plaintext in the repo or committed — inject it via environment variables, a secrets manager, or `sensitive` variables supplied at runtime.
- For any kind of secret, the corresponding variables and outputs MUST always be marked `sensitive = true` so they are never exposed in plan/apply output or logs.

