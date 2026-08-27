---
name: tokenchronicle-archive
description: Archive local Codex activity into the user's private TokenChronicle data directory and verify the generated usage reports.
---

# TokenChronicle Archive

Resolve the plugin root first. If a signed bundled executable exists under `bin/<platform>/`, use the
exact executable matching macOS arm64, macOS x86_64, or Windows x86_64; never execute a binary for a
different platform and never download a replacement. If an installed plugin has no executable for the
current platform, report that the platform client has not been published and stop. For an unpacked development bundle only,
resolve the plugin root relative to this skill and use its bundled Python package: on macOS/Linux use
`PYTHONPATH=<plugin-root>/src python3 -m tokenchronicle.cli <command>`; on Windows use an equivalent
PowerShell environment assignment followed by `py -3 -m tokenchronicle.cli <command>`. Never assume
the Unix command form works on Windows.

1. Run `doctor` and stop if the Codex home is unavailable.
2. Run `archive` and wait for its final completion line and exit code.
3. Never add `--copy-raw`.
4. Verify that the daily token and session usage reports exist under the configured exports directory.
5. Report paths and counts without exposing conversation content.
6. Do not write Codex automation memory, hooks, configuration, or core memory.
