# Uninstall and Data Retention

## Remove program code

For a wheel installation:

```bash
python3 -m pip uninstall tokenchronicle
```

For an unpacked Codex plugin, remove only the installed plugin directory through the same plugin
manager or installation mechanism used to add it.

TokenChronicle does not enable a background service by default. If the user explicitly enabled the
operating-system scheduler, disable it before removing the runtime:

```bash
tokenchronicle schedule disable --confirm-background-schedule
tokenchronicle schedule status
```

This removes the TokenChronicle launchd definition on macOS or the TokenChronicle Task Scheduler entry
on Windows. A foreground `serve` process stops with `Ctrl-C`. Any separately created Codex Automation
must be disabled separately in Codex; TokenChronicle does not remove it implicitly.

## Keep or remove user data

Uninstalling code intentionally preserves configuration, archives, feedback drafts, and receipts.
Run `tokenchronicle doctor` before uninstalling to record the exact paths. Delete those user-owned
directories only after making a backup and confirming that their contents are no longer needed.

Never delete the Codex source directory as part of TokenChronicle removal.

Optional encrypted iCloud snapshots are user-owned files and are not deleted by uninstall. Remove them
only after confirming another usable backup exists.
