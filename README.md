# TokenChronicle Marketplace

![TokenChronicle 词元日志](plugins/tokenchronicle/assets/icon.png)

Public Codex marketplace for TokenChronicle / 词元日志, published by Huahuijian (tianjin)
Technology Co., Ltd.

## Current publication state

The repository marketplace contains TokenChronicle 0.8.1 with signed, notarized macOS clients for
Apple Silicon and Intel. This release fixes standalone daily scheduling, improves incremental archive
performance, and preserves the verifiable activation and privacy-consent boundary introduced in
0.8.0. Windows distribution is not yet available.

This repository contains only:

- Codex marketplace metadata;
- public plugin manifest and Skills;
- signed client binaries and SHA256 checksums;
- installation, privacy, security, token-usage, upgrade, and uninstall documentation.

It will never contain conversations, Codex state, project paths, credentials, feedback records,
private source, server implementation, tests, or private build scripts.

## Installation channels

Register the repository marketplace with the Codex CLI, then open the plugin in Codex desktop:

```sh
codex plugin marketplace add https://github.com/HuaHuiJian/tokenchronicle-marketplace.git
codex plugin add tokenchronicle@tokenchronicle
```

This is a repository-backed marketplace release, not a claim of approval or listing in OpenAI's
built-in catalog. After installation, select **Try now** or start a new Codex task and ask
TokenChronicle to initialize. Installation alone does not archive data or enable daily protection.
安装后请点击 **Try now** 完成首次启用；仅安装插件不会归档数据或启用每日保护。

See [中文使用指南](plugins/tokenchronicle/docs/getting-started.zh-CN.md),
[English guide](plugins/tokenchronicle/docs/getting-started.md), and
[品牌说明 / Brand](plugins/tokenchronicle/docs/brand.md).

The standalone CLI is also available from
[GitHub Releases](https://github.com/HuaHuiJian/tokenchronicle-marketplace/releases) and the official
TokenChronicle Homebrew tap:

```sh
brew install --cask HuaHuiJian/tokenchronicle/tokenchronicle
```

Support: `service@h2me.tech` · Privacy: `privacy@h2me.tech` · Confidential security reports:
`security@h2me.tech`.
