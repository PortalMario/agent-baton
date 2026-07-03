# AGENTS.md — Agent Instructions

These instructions are intended to be applicable to any kind of project that involves some form of code. They are meant to provide a solid instruction base and may need to be extended for even better results.

Follow exactly; all rule changes must be reviewed extra carefully, as they directly shape the code the agent produces.

## Core Principles

- **KISS:** Prefer simpler solutions that solve the problem. Avoid over-engineering, premature abstraction, and unnecessary complexity.
- **Comment sensibly:** Add meaningful comments where intent is not obvious. Explain the *why*, not the trivial *what*.
- **Comment deviations from best practices:** When intentionally deviating from a best practice, add a dedicated separate comment line explaining the reason, if possible.
- **English only:** Write all content — code, comments, identifiers, commit messages, and documentation — in English.

## README Maintenance
- README must contain at least:
  - **Description:** A short summary of what the project/component does.
  - **Usage:** How to run or use it.
  - **Parameters:** A table of invocation/call parameters when it makes sense (e.g. for CLI tools, scripts, or configurable modules).
  - **Contribution:** How to contribute to the project.


## Version Control
- A `.gitignore` tailored to the specific project MUST always exist. Populate it with the ignore patterns relevant to the project's language, tooling, and build artifacts.
- Always push to feature branches whenever possible; never push directly to the main branch.


## Security
- Never commit secrets, credentials, tokens, or private keys.

