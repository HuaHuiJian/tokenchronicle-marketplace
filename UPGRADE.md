# TokenChronicle Upgrade Contract

## Separation of concerns

- Program files are versioned and replaceable.
- User state lives outside the installation directory under `TOKENCHRONICLE_HOME` or the platform default.
- Releases never contain or overwrite archives, reports, settings, feedback drafts, receipts, or logs.

## Safe upgrade sequence

1. Run `tokenchronicle doctor` and record the current version and data directory.
2. Download the new clean release and verify its SHA-256 checksum.
3. Keep the previous program directory as a rollback copy.
4. Install or unpack the new program without moving user state.
5. Run `tokenchronicle doctor`; only then retire the previous program copy.
6. If the local OS scheduler was enabled, rerun `tokenchronicle schedule enable --time HH:MM
   --confirm-background-schedule` from the new runtime so it cannot continue invoking the old runtime.

Configuration schema upgrades add known defaults while preserving explicit consent fields. A code
upgrade never enables automation, feedback transmission, synchronization, or raw evidence copying.

The initial TokenChronicle release embeds
`https://feedback.tokenchronicle.moreglasses.com.cn/v1/feedback` as public routing configuration.
Feedback transmission remains disabled until the user activates a revocable installation credential
and confirms each send.

Do not delete the previous runtime until one manual `tokenchronicle run-daily` and the refreshed
schedule status have both been verified. Upgrades do not create, move, or delete iCloud snapshots.
