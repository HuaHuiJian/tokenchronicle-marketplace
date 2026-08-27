# TokenChronicle compatibility

## Supported runtime

- Python 3.9 through 3.13.
- A local Codex installation whose readable state contains `state_5.sqlite` and rollout records.
- A loopback-capable web browser for the local viewer.

The release CI runs the client tests on Python 3.9 and 3.13 on Linux, and Python 3.13 on current
GitHub-hosted macOS and Windows runners. A matrix entry is engineering evidence only after that exact
workflow completes for the release commit.

## Platform features

| Capability | macOS | Windows | Linux |
| --- | --- | --- | --- |
| Read-only archive, reports, local viewer | Supported | Supported | Supported |
| Local zero-model scheduler | launchd | Task Scheduler | Manual/external scheduler |
| Encrypted iCloud snapshot | Supported with `hdiutil` and iCloud Drive | Not available | Not available |
| Codex Automation | Optional, managed separately in Codex | Optional, managed separately in Codex | Optional, managed separately in Codex |

The default installation enables no scheduler, network feedback, cloud backup, or Codex Automation.

## Compatibility boundary

TokenChronicle observes local Codex storage read-only. `state_5.sqlite` and rollout formats are Codex
implementation details and may change in a future Codex update. `preflight` and `doctor` must fail
closed when required structures are unavailable; the product must never repair or mutate Codex state.

The current Python wheel contains readable proprietary source and is suitable only for authorized
private distribution. The initial public marketplace release supports only independently built,
Developer ID-signed and Apple-notarized macOS arm64 and Intel clients. Windows is added after a
Microsoft Store-certified and Store-signed MSIX path, checksum
publication, and clean-host validation. Unsigned Windows candidates are for compatibility testing only.
