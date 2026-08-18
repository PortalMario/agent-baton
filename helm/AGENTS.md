# CCH Charts Agent Guide

## Workflow and Principles

- Run `helm lint ./<chart>` and `helm template <release> ./<chart>` after every chart change.
- Report failed checks with the command, error, and impact. Never hide failures or disable checks to pass them.
- **KISS:** Prefer simpler solutions that solve the problem. Avoid over-engineering, premature abstraction, and unnecessary complexity.
- **Comment sensibly:** Add meaningful comments where intent is not obvious. Explain the *why*, not the trivial *what*.
- **Comment deviations from best practices:** When intentionally deviating from a best practice, add a dedicated separate comment line explaining the reason, if possible.
- **English only:** Write all content, including code, comments, identifiers, commit messages, and documentation, in English.
- **Security:** Never commit secrets, credentials, tokens, or private keys.
- Configure workloads, services, and access controls according to the principle of least privilege.
- Before exposing a port or service outside the cluster, explicitly ask the user for confirmation.

## README Maintenance

- Every chart must include a README.
- Only update existing documentation when the user explicitly requests it.
- When updating a chart README, keep unrelated content intact and include a short description, installation or usage, required configuration, exposed interfaces, and operational constraints.

## Self-Written Charts

- Keep templates minimal, readable, and limited to resources the chart actually needs.
- Ensure the default configuration runs without user changes, is as secure as practical, and represents a minimal working deployment.
- Run application/init containers unprivileged and non-root; set `allowPrivilegeEscalation: false`, drop all capabilities, and use `RuntimeDefault` seccomp.
- Always define resource requests and limits that fit the service and its expected local workload.
- For every ConfigMap consumed by a workload, add a checksum annotation to that workload's pod template so ConfigMap changes trigger a rollout.
- Put every configurable value and its default in `values.yaml`.
- Add a short YAML comment immediately above every non-obvious value or value group in `values.yaml`.
- Keep `Chart.yaml` complete: `apiVersion`, `name`, `description`, `type`, chart `version`, `appVersion`, `kubeVersion`, and authoritative `sources` are required; add maintainers where known.
- Pin every chart/container version/image to an exact released version; never use `latest` or version ranges.
- Define each container image as one `values.yaml` scalar, for example `image: eclipse-mosquitto:2.0.22`; do not split repository or tag into separate values.

## Dependency Charts

- Use only official Helm charts published by the upstream project or its clearly verified official organization.
- If no official, secure, unambiguous upstream chart exists, stop and ask the user before selecting a third-party chart or writing a replacement.
- Pin dependency chart versions exactly in `Chart.yaml` and commit the matching `Chart.lock`. Do not commit generated dependency archives under `charts/`; recreate them with `helm dependency build`.
- Pin images configured through dependency values to exact released versions; never use `latest` or version ranges.
- When the user requests a dependency chart update, research the upstream release notes for breaking changes and verify whether the existing chart configuration remains compatible; report any required configuration changes.
