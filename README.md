# TokenChronicle Marketplace

Public Codex marketplace for TokenChronicle / 词元日志, published by Huahuijian (tianjin)
Technology Co., Ltd.

## Current publication state

The catalog is intentionally empty while the closed-source signed client is being prepared. The
current Python ZIP and wheel are not published here because they contain readable source code.

Once a signed binary release passes clean-environment validation, this repository will contain only:

- Codex marketplace metadata;
- public plugin manifest and Skills;
- signed client binaries and SHA256 checksums;
- installation, privacy, security, token-usage, upgrade, and uninstall documentation.

It will never contain conversations, Codex state, project paths, credentials, feedback records,
private source, server implementation, tests, or private build scripts.

## Future installation

After the first plugin release is published:

```bash
codex plugin marketplace add https://github.com/HuaHuiJian/tokenchronicle-marketplace
codex plugin add tokenchronicle@tokenchronicle
```

See `docs/getting-started.zh-CN.md` or `docs/getting-started.md` for product and privacy guidance.

Support: `service@h2me.tech` · Privacy: `privacy@h2me.tech` · Confidential security reports:
`security@h2me.tech`.
