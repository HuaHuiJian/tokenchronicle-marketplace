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

See `docs/compatibility.md` for the supported/tested matrix and platform-specific feature limits.

Not required:

- a dedicated project or working directory;
- Node.js, Docker, or a database server;
- third-party Python packages;
- network access for archive, analysis, or local viewing.

Run the read-only environment check before installation:

```bash
PYTHONPATH=src python3 -m tokenchronicle.cli preflight
```

Run `guide` at any time for a short value statement, requirements, choices, and first-use sequence:

```bash
PYTHONPATH=src python3 -m tokenchronicle.cli guide
```

## Choose where data lives

No manual directory creation is necessary. TokenChronicle creates private directories when setup is
accepted.

When setup is launched from a sandboxed Codex environment, Codex or macOS may ask the user to approve
writing to the selected application data directory. The user should see the exact path before deciding.
Declining leaves Codex and the selected directory unchanged; the user may choose another writable data
location and run preflight again.

On macOS, the default user-owned location is:

```text
~/Library/Application Support/TokenChronicle/
├── config/config.json
├── data/exports/
├── data/feedback/drafts/
├── data/feedback/receipts/
├── state/daily-run.json
├── state/locks/
├── logs/daily/
├── backups/
└── migration/
```

Setup distinguishes the installed plugin or runtime, operational application state, and the durable
archive library. In a Codex plugin process, operational state uses the host-provided `PLUGIN_DATA`
directory. A standalone install uses the operating-system application-data directory.

Use the default locations:

```bash
PYTHONPATH=src python3 -m tokenchronicle.cli setup --accept-privacy
```

Or choose a Codex source, data parent directory, and viewer port:

```bash
PYTHONPATH=src python3 -m tokenchronicle.cli setup --accept-privacy \
  --codex-home /path/to/.codex \
  --archive-dir /path/to/TokenChronicleArchive \
  --port 8877
```

Alternatively, set `TOKENCHRONICLE_HOME` to relocate configuration and all default user state. The
same environment variable must be present for later commands.

A Documents archive is visible and portable, but iCloud, enterprise synchronization, search indexing,
or backup software may process it. The default application-data archive is less visible. Setup shows
the exact paths before consent, and a location choice never enables scheduling, feedback, or migration.

## Understand the choices

Setup does not enable scheduled Automation, feedback transmission, synchronization, or raw evidence
copying.

- **Manual archive:** run only when needed; no model Token use is introduced by the local Python command.
- **Codex daily Automation:** optional and separately authorized. Plan for `0.7M-2.0M` total Tokens
  per run. See `docs/token-usage.md` before enabling it.
- **Feedback:** optional. Drafts remain local unless the user configures an HTTPS receiver and
  confirms an individual submission.
- **Raw evidence:** disabled. Normal archives store redacted events and do not copy the source rollout.
- **Encrypted iCloud snapshot:** disabled and manual only. It uses local CPU and disk, not model tokens.

## Optional encrypted iCloud snapshot

The live archive stays local. On macOS, TokenChronicle can build and verify an AES-256 encrypted DMG
locally before publishing the immutable snapshot to iCloud Drive. iCloud synchronization is not an
independent backup; Advanced Data Protection and an additional backup are recommended.

```bash
tokenchronicle backup status
tokenchronicle backup create --confirm-cloud-backup
```

Run the create command in the user's own interactive terminal. The password is prompted twice and must
never be placed in chat, command arguments, configuration, or scripts. Version 0.7.1 does not schedule
cloud snapshots automatically.

## First use

```bash
PYTHONPATH=src python3 -m tokenchronicle.cli doctor
PYTHONPATH=src python3 -m tokenchronicle.cli archive
PYTHONPATH=src python3 -m tokenchronicle.cli serve
```

Then open `http://127.0.0.1:8777/`. The viewer remains local by default.

Use `memory-daily` when local memory lifecycle reports are wanted:

```bash
PYTHONPATH=src python3 -m tokenchronicle.cli memory-daily
```

After the first manual archive and viewer acceptance pass, a user may explicitly enable the bundled
zero-model-token OS scheduler:

```bash
tokenchronicle run-daily
tokenchronicle schedule enable --time 03:20 --confirm-background-schedule
tokenchronicle schedule status
```

Observe at least seven successful daily runs before disabling a legacy Codex Automation. Do not keep
both schedulers active long term.

## Storage and upgrades

Conversation archives can become much larger than the program. `doctor` reports the current archive
size, free space, chosen paths, Automation state, feedback state, and whether the configured port is
available for a new viewer process. An unavailable port can also mean TokenChronicle is already running;
it does not by itself mean the installation is damaged.

Program files and user data are separate. Upgrade by replacing only the plugin or application code,
then run `doctor`; do not move archives into the release directory. See `UPGRADE.md` for the rollback
sequence.
