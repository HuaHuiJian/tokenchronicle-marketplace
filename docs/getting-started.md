# TokenChronicle Getting Started

## What TokenChronicle gives you

TokenChronicle creates a private, searchable history of local Codex work. It preserves user inputs,
Codex-visible replies, process records, file-oriented evidence, and Token trends so a user can review
decisions, audit how work was completed, recover useful ideas, and build future personal insights.

It is an external, read-only observer of Codex. It does not require a dedicated Codex project and it
does not write files into the projects being analyzed.

## Before installation

Required:

- no separate Python installation for the signed Codex Marketplace plugin;
- Python 3.9 or newer only for source development or internal ZIP/wheel validation;
- a local Codex installation that has already created `state_5.sqlite`;
- read access to the Codex data directory;
- write access to a user-selected TokenChronicle data directory;
- enough local disk space for archives. Start with at least 5 GB free for a regular user and monitor
  growth after the first archive.

Not required:

- a dedicated project or working directory;
- Node.js, Docker, or a database server;
- third-party Python packages;
- network access for archive, analysis, or local viewing.

See `docs/compatibility.md` for the supported/tested matrix and platform feature limits.

Run the read-only environment check before installation:

```bash
tokenchronicle preflight
```

Run `guide` at any time for a short value statement, requirements, choices, and first-use sequence:

```bash
tokenchronicle guide
```

## Choose where data lives

No manual directory creation is necessary. TokenChronicle creates private directories when setup is
accepted.

When setup is launched from a sandboxed Codex environment, Codex or macOS may ask the user to approve
writing to the selected application data directory. The user should see the exact path before deciding.
Declining leaves Codex and the selected directory unchanged; the user may choose another writable data
location and run preflight again.

Codex uses the host-provided `PLUGIN_DATA` directory for plugin operational state. The durable archive
library is a separate user choice. For a standalone macOS installation, the default location is:

```text
~/Library/Application Support/TokenChronicle/
├── config/config.json
├── data/exports/
├── data/feedback/drafts/
├── data/feedback/receipts/
├── logs/
└── backups/
```

Use the default locations:

```bash
tokenchronicle setup --accept-privacy
```

Or choose a Codex source, exact durable archive library, and viewer port:

```bash
tokenchronicle setup --accept-privacy \
  --codex-home /path/to/.codex \
  --archive-dir ~/Documents/TokenChronicle \
  --port 8877
```

Alternatively, set `TOKENCHRONICLE_HOME` to relocate configuration and all default user state. The
same environment variable must be present for later commands.

## Understand the choices

Setup does not enable scheduled Automation, feedback transmission, synchronization, or raw evidence
copying.

- **Manual archive:** run only when needed; no model Token use is introduced by the local Python command.
- **Local OS schedule:** optional after a successful first archive; it executes deterministic local
  commands and introduces no model Token use.
- **Codex daily Automation:** optional and separately authorized. Plan for `0.7M-2.0M` total Tokens
  per run. See `docs/token-usage.md` before enabling it.
- **Feedback:** optional. Drafts remain local unless the user configures an HTTPS receiver and
  confirms an individual submission.
- **Raw evidence:** disabled. Normal archives store redacted events and do not copy the source rollout.

## First use

```bash
tokenchronicle doctor
tokenchronicle archive
tokenchronicle serve
```

Then open `http://127.0.0.1:8777/`. The viewer remains local by default.

Use `memory-daily` when local memory lifecycle reports are wanted:

```bash
tokenchronicle memory-daily
```

## Storage and upgrades

Conversation archives can become much larger than the program. `doctor` reports the current archive
size, free space, chosen paths, Automation state, feedback state, and whether the configured port is
available for a new viewer process. An unavailable port can also mean TokenChronicle is already running;
it does not by itself mean the installation is damaged.

Program files and user data are separate. Upgrade by replacing only the plugin or application code,
then run `doctor`; do not move archives into the release directory. See `UPGRADE.md` for the rollback
sequence.
