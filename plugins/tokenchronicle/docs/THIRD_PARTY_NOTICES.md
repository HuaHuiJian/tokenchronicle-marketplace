# Third-Party Notices

The TokenChronicle 0.7.0 client wheel declares no third-party Python runtime dependencies and does not
bundle a Python interpreter. It uses Python standard-library modules and, on macOS, invokes the system
`hdiutil` utility for an explicitly requested encrypted snapshot.

Codex, OpenAI, ChatGPT, macOS, iCloud, Windows, and Linux are names or marks of their respective owners.
Their software and services are governed by their own terms.

The optional TokenChronicle feedback server is built and distributed from the separate private
`tokenchronicle-server` repository. Its Alibaba Cloud and Tencent Cloud adapters include npm dependencies
under their respective upstream licenses. Every server release must be built from its lockfiles, pass the
production dependency audit, and carry a complete lockfile-derived license inventory and SBOM before
distribution.

This file must be updated whenever a client dependency, embedded runtime, font, image, icon, library, or
other redistributable third-party asset is added.

Standalone signed clients additionally embed a Python runtime and the PyInstaller bootloader. Their
exact upstream `LICENSE` and `COPYING.txt` files are copied from the pinned build environment into every
platform artifact. PyInstaller is used under GPL-2.0-or-later with its Bootloader Exception; its bundled
runtime hooks carry their stated upstream licenses.
