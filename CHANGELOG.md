# Changelog

## 0.8.1 - 2026-09-09

- Fixed frozen Marketplace clients so macOS launchd and Windows Task Scheduler invoke the standalone executable directly.
- Prevented pathological redaction time on large conversation data.
- Reused complete, unchanged session exports while fully rebuilding new or updated sessions.
- Verified a real macOS launchd run outside Codex with exit code 0 and zero model-token use.

## 0.8.0 - 2026-09-09

- Added a read-only readiness state machine for every onboarding and maintenance entry point.
- Blocked archive, viewer, memory, and daily-run operations before configuration and privacy consent.
- Required the guided flow to finish with either verified OS scheduling or explicit manual-only mode.
- Added cross-platform release tests and clearer bilingual activation documentation.

## 0.7.3 - 2026-09-02

- Changed the Marketplace **Try now** action into a guided, read-only-first initialization flow.
- Added an explicit choice summary and natural-language privacy confirmation before any setup write.
- Kept scheduling, Automation, feedback, cloud backup, and historical migration under separate consent.

## 0.7.0 - 2026-08-20

- Added explicit macOS encrypted iCloud snapshot guidance and lifecycle boundaries.
- Added compatibility, NOTICE, and third-party component disclosures.

## 0.6.0 - 2026-08-20

- Added opt-in macOS launchd and Windows Task Scheduler support with zero model-token use.
- Added platform application-data paths and clearer runtime/archive separation.
- Kept all scheduling, feedback, backup, and historical migration disabled by default.

## 0.5.0 - 2026-08-13

- Added the distributable TokenChronicle 词元日志 Codex plugin and Python CLI.
- Added local-first bilingual onboarding, privacy consent, diagnostics, and usage disclosure.
- Added isolated persistent user storage and data-safe configuration migration.
- Added explicit, credential-bound feedback with Alibaba Cloud, Tencent Cloud, and Cloudflare adapters.
- Added deterministic clean release archives, offline-installable wheel packaging, checksums, and release validation.
