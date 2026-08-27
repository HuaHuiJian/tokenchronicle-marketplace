---
name: tokenchronicle-setup
description: Initialize, diagnose, or open the local TokenChronicle Codex activity archive without importing any packaged user data.
---

# TokenChronicle Setup

Resolve the plugin root first. If a signed bundled executable exists under `bin/<platform>/`, use the
exact executable matching macOS arm64, macOS x86_64, or Windows x86_64; never execute a binary for a
different platform and never download a replacement. A public plugin release may support only a subset
of platforms. If this installed plugin has no executable for the current platform, report that the
platform client has not been published and stop; do not download or substitute an unrelated launcher.
For an unpacked development bundle only,
resolve the plugin root relative to this skill and use its bundled Python package. Use `python3` plus
`PYTHONPATH` on macOS/Linux and `py -3` plus PowerShell environment syntax on Windows. Never copy a
Unix environment-assignment command unchanged into Windows.

## Initialize

1. Run `guide`, then explain that TokenChronicle privately preserves searchable Codex inputs, replies,
   process evidence, and usage trends for review, audit, idea recovery, and future personal insights.
2. Explain that no dedicated project or working directory is required. TokenChronicle reads the chosen
   Codex home read-only and writes only to its separate user-owned application data directory.
   Distinguish the installed plugin/runtime, operational application state, and durable archive library.
   Use host-provided `PLUGIN_DATA` for plugin operational state when available.
3. Run the read-only `preflight` command. Report Python support, Codex state availability, selected data
   path, current archive size, free disk space, viewer port status, and every failing check.
4. Give the user a real choice before setup:
   - accept the operating-system defaults;
   - choose a different Codex home with `--codex-home`;
   - choose an exact durable archive library with `--archive-dir`;
   - choose a different local port with `--port`.
   - follow the system language with `--language auto`, or select Simplified Chinese with `zh-CN`
     or English with `en`.
   Do not ask the user to create these directories manually.
   Explain that Documents is visible and portable but may be processed by iCloud, enterprise sync,
   search indexing, or backup software. The default application-data location is less visible.
   If the selected data directory is outside the current sandbox, show the exact path and request the
   required filesystem approval. If the user declines, do not work around the decision; offer another
   user-selected writable location and rerun `preflight`.
5. Explain that all scheduling is disabled by default, so the default model-token use is `0/day`.
   Recommend the bundled local OS scheduler after a successful manual archive; it runs deterministic
   commands with zero model tokens and requires `--confirm-background-schedule`. If the user instead
   asks to create a daily Codex Automation, disclose the planning estimate: `0.7M-2.0M`
   total tokens per run, commonly `1.0M-1.5M`, or about `21M-60M` over 30 daily runs. State that
   cached input is included, the estimate is not a billing quote, and first or broad scans can reach
   approximately `1.5M-4.0M`.
6. Explain that archives can grow to multiple GB and recommend at least 5 GB of free space before the
   first archive. A low-space warning informs the user but does not silently delete data.
7. Ask for explicit privacy acceptance before initialization. Ask separately before enabling the local
   OS scheduler, creating Codex Automation, or enabling feedback transmission; one consent never implies another.
8. After the user selects paths and accepts privacy, run `setup --accept-privacy` with the selected flags.
   Do not enable optional features.
9. Run `doctor` and `usage-notice`, report every failing check, then offer the first-use sequence:
   `archive`, `serve`, and optionally `memory-daily`. Only after those pass, offer `run-daily`, followed
   by an explicitly consented `schedule enable`. Keep any legacy Codex Automation active until the new
   schedule has a verified run, then avoid running both schedulers long term.
   Treat an unavailable viewer port as either an already-running viewer or a conflict; check before
   recommending a different port.
10. Explain that the Web language can be switched at any time. Translate only product UI and guidance;
    preserve user inputs, session titles, Codex responses, process evidence, and archived files verbatim.

## Safety boundaries

- Do not copy packaged sample conversations because the plugin contains none.
- Do not modify Codex hooks, configuration, memories, or automation state.
- Do not enable network feedback or synchronization without explicit consent.
- Keep all generated data in the configured TokenChronicle application data directory.
