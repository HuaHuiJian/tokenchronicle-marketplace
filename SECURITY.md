# Security Policy

## Supported version

Security fixes are applied to the latest published TokenChronicle release.

## Reporting

Report suspected vulnerabilities privately to `security@h2me.tech`. Do not place exploit details,
secrets, conversation content, local paths, or credentials in a public issue or the in-product
feedback flow. For ordinary product support, use `service@h2me.tech` instead.

The publisher must monitor `security@h2me.tech`, restrict mailbox access, use MFA, and exercise the
incident-response path before the catalog becomes installable. Product feedback and public issues are
not substitutes for confidential exploit reporting.

## Client boundary

- Codex sources are read-only; archives and configuration live outside the installed program.
- The viewer binds to loopback by default and should not be exposed directly to a network.
- Automation and feedback are disabled by default.
- Installation and server administration credentials must never be included in a release artifact.

See `PRIVACY.md` and `FEEDBACK_PROTOCOL.md` for the public client and feedback boundaries. Receiver
implementation and operator controls are maintained separately in the private server repository.
