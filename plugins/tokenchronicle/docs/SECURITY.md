# Security Policy

## Supported version

Security fixes are applied to the latest published TokenChronicle release.

## Reporting

Report suspected vulnerabilities privately to `security@h2me.tech` or through
[GitHub Private Vulnerability Reporting](https://github.com/HuaHuiJian/tokenchronicle-marketplace/security/advisories/new).
Do not place exploit details, secrets, conversation content, local paths, or credentials in a public
issue or the in-product feedback flow. For ordinary product support, use `service@h2me.tech` instead.

## Client boundary

- Codex sources are read-only; archives and configuration live outside the installed program.
- The viewer binds to loopback by default and should not be exposed directly to a network.
- Automation and feedback are disabled by default.
- Installation and server administration credentials must never be included in a release artifact.

See `PRIVACY.md`, `FEEDBACK_PROTOCOL.md`, and the separately maintained private
`HuaHuiJian/tokenchronicle-server` security baseline for the full boundaries.

The publisher must monitor `security@h2me.tech`, restrict mailbox access, use MFA, and exercise the
incident-response path before public release. Public GitHub issues and product feedback must not be
used to transmit exploit details, credentials, conversations, or local paths.
