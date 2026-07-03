# AGENTS.md — Agent Instructions

Authoritative rules for cloud-init prsioning. 

Follow exactly; all rule changes must be reviewed extra carefully, as they directly shape the code the agent produces.

## Core Principles

- **KISS:** Prefer simpler solutions that solve the problem. Avoid over-engineering, premature abstraction, and unnecessary complexity.
- **Comment sensibly:** Add meaningful comments where intent is not obvious. Explain the *why*, not the trivial *what*.
- **Comment deviations from best practices:** When intentionally deviating from a best practice, add a dedicated separate comment line explaining the reason, if possible.
- **English only:** Write all content — code, comments, identifiers, commit messages, and documentation — in English.
- Make configs idempotent and safely re-runnable.
- Use cloud-init meta-data to define instance variables (hostname, instance-id, etc.) and template default.user-data via Jinja2 ({{ ds.meta_data.variable }}) against it
- Parametrize every occurrence of Distro suite names and version numbers in user-data via meta-data variables (e.g. use {{ ds.meta_data['ubuntu-suite'] }} instead of hardcoding 'resolute'). 
- Keep configs as distro-independent as possible; avoid distro-specific assumptions and prefer portable parametrization over hardcoding a single distribution.
- Output must pass cloud-init schema `--config-file`


## Version Control

- A `.gitignore` tailored to the specific project MUST always exist. Populate it with the ignore patterns relevant to the project's language, tooling, and build artifacts.
- Always push to feature branches whenever possible; never push directly to the main branch.

## Security

- Never commit secrets, credentials, tokens, or private keys.
- Never put plaintext secrets into cloud-init files (user-data/meta-data). If a secret is unavoidable, only include it in hashed form (e.g. `passwd` as a hashed password), never in cleartext.


## README maintenance

README.md must always contain the following sections:

- *Meta-data variables* — a table of every key in meta-data with a description and an example value
- *Usage* — instructions for provisioning a new instance and for forcing a re-run on an existing instance
- *Debugging* — commands for inspecting cloud-init status, live logs, per-module logs.
