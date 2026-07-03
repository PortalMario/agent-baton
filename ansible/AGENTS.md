# Ansible Repo — Agent Instructions

Authoritative rules for this ansible related repo.

Follow exactly; all rule changes must be reviewed extra carefully, as they directly shape the code the agent produces.

## Principles

- **KISS:** Prefer simpler solutions that solve the problem. Avoid over-engineering, premature abstraction, and unnecessary complexity.
- **Comment sensibly:** Add meaningful comments where intent is not obvious. Explain the *why*, not the trivial *what*.
- **Comment deviations from best practices:** When intentionally deviating from a best practice, add a dedicated separate comment line explaining the reason, if possible.
- **English only:** Write all content — code, comments, identifiers, commit messages, and documentation — in English.

## Version Control

- A `.gitignore` tailored to this repo MUST always exist. Populate it with the ignore patterns relevant to Ansible, its tooling, and build artifacts.
- Always push to feature branches whenever possible; never push directly to the main branch.

## Inventories

- Inventories MUST always be in YAML format (`.yml`) — never use INI or any other inventory format.

## Dependencies

- Galaxy roles and collections go in `requirements.yml`; Python dependencies go in `requirements.txt`.
- Every entry in `requirements.yml` and `requirements.txt` MUST always be version-pinned — never leave a dependency unpinned or floating.
- The repo `README.md` MUST state the minimum required Python version for the Ansible project.

## Roles

- Every role variable must be declared in `defaults/main.yml` with a sensible default — never only in `vars/`, inline, or implicitly via `group_vars`/`host_vars`. Defaults must be overridable from `group_vars`/`host_vars`.
- Portable: no hardcoded IPs/hostnames — use variables with sensible defaults.
- For list-managed resources (users, keys, configs), implement removal of dropped entries when the use case calls for it.
- Each role needs a `README.md`: purpose, defaults table, usage example.

## Code Quality

- Must pass `ansible-lint` (config: `.ansible-lint`) and run cleanly with `--check`.
- Check-mode exceptions: `check_mode: false` with a comment explaining why.
- Tags: question whether one is needed — adding a tag means adding it everywhere consistently.
- Inline file content via `ansible.builtin.copy` `content:` only for small blobs; larger content goes in `files/` or `templates/` via `src:`.
- Use handlers for service restarts; notify only on actual change. Never restart unconditionally.
- Pin all package and container image versions; upgrades happen by changing the pinned variable. Prevent the unintended `apt upgrade` of pinned packages.
- Software versions on the target system must be controllable via an Ansible variable (e.g. `openvpn_version: v1.2.4`). Changing it (e.g. to `v1.2.5`) must perform the upgrade AND remove the old version, leaving no stale artifacts behind.

## Secrets & `no_log`

- `no_log: true` on every task, `register`, and loop touching secrets/passwords/tokens.
- Secrets never land as plaintext on a target host — hash where applicable.

## Production Security

**Production infra managed by ansible — least privilege, restrictive permissions, no plaintext secrets, validate inputs at boundaries, no sensitive data in logs/debug.**

## Execution

- **Everything must also be runnable locally from the Ansible controller (i.e. the local machine running Ansible), regardless of which host acts as the controller.**
- Locally, secrets (e.g.: OpenBao `role_id`, `secret_id`, etc.) must be set via `ansible-vault`.

## Scripts & Code

- Single Python or Bash scripts allowed when tied to a role or task.
- No full projects, packages, or dependency trees in this ansible-repo.

## README Maintenance

- README must contain at least:
  - **Description:** A short summary of what the project/component does.
  - **Usage:** How to run or use it.
  - **Parameters:** A table of invocation/call parameters when it makes sense (e.g. for CLI tools, scripts, or configurable modules).
  - **Contribution:** How to contribute to the project.
