# TokenChronicle Privacy Contract

## Non-negotiable boundary

Program releases contain executable code, static presentation assets, schemas, migrations,
documentation, and synthetic test fixtures only. They must never contain user conversations,
rollout events, thread identifiers, local settings, local memory files, personal paths, logs,
screenshots, generated reports, or credentials.

## Local data ownership

User state is stored outside the installation directory. The default location is the operating
system's per-user application data directory and can be overridden with `TOKENCHRONICLE_HOME`.

The following are user-owned data and are never release inputs:

- configuration and consent choices;
- archived conversations and process records;
- indexes, statistics, summaries, and memory candidates;
- logs, backups, screenshots, feedback drafts, and feedback receipts.

Feedback drafts and receipts are persistent user-owned records. TokenChronicle does not expire or remove
them automatically; the user may delete them explicitly from local application data when desired.

Codex plugin execution uses the host-provided `PLUGIN_DATA` directory when available. Standalone
execution uses the operating system's per-user application-data location. Configuration, locks, logs,
feedback, and credentials remain operational state. Durable archives may use that private default or
an exact user-selected archive directory such as `Documents/TokenChronicle`.

Before choosing Documents, setup must explain that iCloud, enterprise synchronization, search indexing,
or backup software may process that directory. Selecting a location never enables scheduling, Codex
Automation, feedback transmission, or historical migration.

## Network behavior

The core archive and viewer require no network access. Feedback and future synchronization remain
disabled by default. Any transmission must show the exact payload, require explicit confirmation,
and exclude conversation content unless the user deliberately attaches it.

Feedback drafts contain only product version, page name, category, optional rating, user-written
message, timestamp, and an explicit privacy declaration. Drafts remain local unless the user first
configures an HTTPS receiver, reviews the exact payload, and confirms that individual submission.
Enabling the receiver never enables background or batch transmission.

## Optional encrypted iCloud snapshots

iCloud backup is disabled by default and creates no automatic schedule. On explicit request, the macOS
client builds an AES-256 encrypted DMG locally, verifies it, and only then publishes the immutable
snapshot to the user's iCloud Drive. The live archive and operational state remain local.

The password is entered interactively, passed to the macOS system utility over standard input, and is
never stored, logged, included in a command argument, or transmitted by TokenChronicle. A detailed
per-file manifest stays inside the encrypted image; the external receipt contains bounded aggregate
counts, image checksum, and verification state only. iCloud synchronization is not an independent backup.

The optional feedback credential identifies only a random installation instance. It is not derived
from a username, device serial number, project, Codex account, or hardware fingerprint. It is stored
only in private user-local configuration and grants write-only feedback access.

The compatible TokenChronicle receiver persists accepted feedback by default because this dataset is intentionally
small and contains only the privacy-bounded feedback envelope. Automatic expiry is disabled unless
the operator explicitly adopts a positive retention period. Administrators retain explicit deletion
and installation-revocation controls.

## Codex boundary

TokenChronicle observes supported local Codex artifacts read-only. It does not modify Codex core files,
hooks, configuration, memories, or automation state without a separate explicit user action.
