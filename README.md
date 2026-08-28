# TokenChronicle Marketplace

![TokenChronicle 词元日志](plugins/tokenchronicle/assets/icon.png)

Public Codex marketplace for TokenChronicle / 词元日志, published by Huahuijian (tianjin)
Technology Co., Ltd.

## Current publication state

The repository marketplace now contains the TokenChronicle plugin and signed, notarized macOS
0.7.2 clients for Apple Silicon and Intel. The branding update adds the time-ring and journal Logo
without changing the client binaries. Windows distribution is not yet available.

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
built-in catalog. Start a new Codex task after installation and ask TokenChronicle to run setup;
it will show the Codex source, operational-state directory, and archive-library choices before
initializing user data. 安装后新建任务运行初始化引导，确认隐私授权和归档目录后再开始使用。

See [中文使用指南](plugins/tokenchronicle/docs/getting-started.zh-CN.md),
[English guide](plugins/tokenchronicle/docs/getting-started.md), and
[品牌说明 / Brand](plugins/tokenchronicle/docs/brand.md).

Support: `service@h2me.tech` · Privacy: `privacy@h2me.tech` · Confidential security reports:
`security@h2me.tech`.
