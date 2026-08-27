# TokenChronicle Token Usage Disclosure

## The default is zero model tokens

A fresh TokenChronicle installation does not create or enable a scheduled Automation. Opening the
local viewer, browsing existing archives, and running the deterministic Python archive commands do
not invoke a model. The default background model-token usage is therefore **0 tokens per day**.

TokenChronicle must show this disclosure before asking a user to create a scheduled Codex
Automation. The user may decline and continue to use manual local archive and viewer commands.

## Optional Codex daily Automation estimate

When a user explicitly creates a daily Codex Automation, the Automation invokes a model to operate
the local commands and verify their results. The following conservative planning ranges come from an
anonymized pre-release benchmark. No source conversation, account, path, date-level record, or user
setting from that benchmark is included in the product or release artifacts.

| Measure | Total tokens |
| --- | ---: |
| Practical planning range per run | 0.7M-2.0M |
| Common daily range | 1.0M-1.5M |
| Estimated 30-day range | 21M-60M |
| First or unusually broad scan | 1.5M-4.0M |

These figures are planning estimates, not a guaranteed allowance or price. The practical daily range
is intentionally broad so users can budget for changing archive size and run behavior.

## What the estimate means

- The source metric is Codex `total_tokens`, including cached input tokens.
- `total_tokens` is not the same as billable cost. Billing and plan treatment can vary by model,
  product plan, and cache policy.
- Usage grows with the number and size of archived tasks, context supplied to the Automation, tool
  output, selected model, cache behavior, `archive_limit`, and whether archived tasks are included.
- A first scan or a recovery run may consume more than a routine incremental run.
- Multiple scheduled runs per day multiply the expected usage.

Run `tokenchronicle usage-notice` at any time for the machine-readable disclosure. `tokenchronicle
doctor` also reports the current Automation state and the same estimate. The TokenChronicle overview
shows actual historical Token usage after archives are available.

## Zero-model scheduling option

The underlying archive and memory report commands are deterministic local Python operations. An
operating-system scheduler can run them directly with an expected model-token cost of zero, while
still using local CPU, memory, and disk. TokenChronicle bundles adapters for macOS `launchd` and
Windows Task Scheduler. They remain disabled until the user explicitly runs `tokenchronicle schedule
enable --time HH:MM --confirm-background-schedule`. The schedule runs `tokenchronicle run-daily`,
writes a local status record and log, and never creates a Codex task.

Manual encrypted iCloud snapshots also use zero model tokens. A full snapshot uses local CPU,
temporary disk, upload bandwidth, and iCloud capacity that is typically close to the archive size.
