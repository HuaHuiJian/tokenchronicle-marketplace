# Uninstall and Data Retention

## Remove program code

For a wheel installation:

```bash
python3 -m pip uninstall tokenchronicle
```

For an unpacked Codex plugin, remove only the installed plugin directory through the same plugin
manager or installation mechanism used to add it.

TokenChronicle does not install a background operating-system service. A foreground `serve` process
stops with `Ctrl-C`, and a scheduled Codex Automation must be disabled separately in Codex.

## Keep or remove user data

Uninstalling code intentionally preserves configuration, archives, feedback drafts, and receipts.
Run `tokenchronicle doctor` before uninstalling to record the exact paths. Delete those user-owned
directories only after making a backup and confirming that their contents are no longer needed.

Never delete the Codex source directory as part of TokenChronicle removal.
