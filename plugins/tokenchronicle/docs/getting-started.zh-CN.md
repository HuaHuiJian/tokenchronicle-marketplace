# TokenChronicle 词元日志使用指南

## 产品价值

TokenChronicle 词元日志是一套本地优先的 Codex 使用记录、整理与回溯系统。它把用户输入、
Codex 可见回复、任务过程、文件化证据与 Token 使用趋势保存在用户自己的电脑中，帮助用户：

- 回看一个任务是如何完成的；
- 审计工具调用、过程记录和最终回答；
- 找回历史输入、思路与灵感；
- 统计项目、会话和时间维度的 Token 使用；
- 为未来的工作领域分析、个人偏好和记忆传承提供可靠资料。

它运行在 Codex 外围，以只读方式观察 Codex 数据，不要求用户创建专用项目，也不会向被分析的
项目目录写文件。系统只翻译产品界面，不翻译用户输入、会话标题、Codex 回复或审计证据。

## 语言选择

安装时可以选择：

```bash
tokenchronicle setup --accept-privacy --language zh-CN
tokenchronicle setup --accept-privacy --language en
tokenchronicle setup --accept-privacy --language auto
```

`auto` 会跟随浏览器或系统语言。Web 页面右上角可以随时在“简体中文”和“English”之间切换，
选择保存在浏览器本地，不会上传。

## 安装前要求

- 从 Codex Marketplace 安装正式插件时，无需用户另行安装 Python；
- 只有源码开发或内部 ZIP/wheel 验证才要求 Python 3.9 或更高版本；
- 本机已使用过 Codex，并存在 `state_5.sqlite`；
- 可以读取 Codex 数据目录；
- 可以写入用户选择的 TokenChronicle 数据目录；
- 首次归档建议至少预留 5 GB 可用磁盘空间。

支持与已测试的系统矩阵、各平台功能差异见 `docs/compatibility.zh-CN.md`。

不需要 Node.js、Docker、独立数据库、第三方 Python 包或专门工作目录。归档、分析和本地浏览
不需要联网。

安装前可以先只读检查，不创建任何目录：

```bash
tokenchronicle guide
tokenchronicle preflight
```

## 目录选择

安装流程会分别展示三类位置：

1. 插件或独立运行时：程序代码，只随产品升级替换。
2. 运行状态目录：配置、锁、日志、反馈和凭证。
3. 长期档案库：会话、过程文件、报表和记忆。

macOS 默认数据目录为：

```text
~/Library/Application Support/TokenChronicle/
```

Windows 默认使用 `%LOCALAPPDATA%\TokenChronicle\`，Linux 默认使用
`$XDG_DATA_HOME/tokenchronicle/` 或 `~/.local/share/tokenchronicle/`。目录按职责分开：

```text
TokenChronicle/
├── config/config.json
├── data/exports/
├── data/feedback/{drafts,receipts}/
├── state/{daily-run.json,locks/}
├── logs/daily/
├── backups/
└── migration/
```

Codex 插件运行时优先使用官方提供的 `PLUGIN_DATA` 保存运行状态；独立安装使用上述操作系统目录。
用户不需要手工创建。长期档案库可以保留在默认私有位置，也可以精确选择：

```bash
tokenchronicle setup --accept-privacy \
  --codex-home /path/to/.codex \
  --archive-dir ~/Documents/TokenChronicle \
  --language zh-CN \
  --port 8877
```

选择 `Documents` 更便于人工查看、迁移和备份，但可能被 iCloud、企业同步、搜索索引或备份软件
处理；默认应用数据目录更私密。该目录选择不会开启调度、Codex Automation、反馈或历史迁移。

如果 Codex 或 macOS 要求访问所选目录，安装流程必须先展示准确路径并征得用户授权。用户拒绝后，
TokenChronicle 不会绕过决定，可以改选其他可写目录并重新预检。

## 可选功能和成本

- 手工归档不会引入模型 Token 消耗。
- 每日 Codex Automation 默认关闭，产品默认后台消耗为 `0 Token/日`。
- 显式启用每日 Automation 后，建议按每次 `0.7M-2.0M total_tokens` 规划。
- 反馈默认关闭，每次联网发送都必须单独确认。
- 默认不复制未脱敏的原始 rollout 文件。
- iCloud 加密快照默认关闭；手工创建不消耗模型 Token，但需要接近档案体量的临时空间和 iCloud 容量。

完整估算口径见 `docs/token-usage.md`。

## 可选 iCloud 加密快照

实时档案始终留在本地。TokenChronicle 可以在本地生成 AES-256 加密 DMG、校验成功后再写入
iCloud Drive。iCloud 是同步服务，不是独立备份；建议启用 Apple 高级数据保护并保留其他备份。

```bash
tokenchronicle backup status
tokenchronicle backup create --confirm-cloud-backup
```

第二条命令会在用户自己的交互终端中两次安全询问密码。不要在 Codex 对话、命令参数、配置或脚本
中提供密码。当前版本只支持手工快照，不会自动创建周期备份。

## 第一次使用

```bash
tokenchronicle doctor
tokenchronicle archive
tokenchronicle serve
```

随后打开 `http://127.0.0.1:8777/`。需要每日记忆生命周期报告时，再运行：

```bash
tokenchronicle memory-daily
```

首次手工归档和页面验收成功后，可以再单独启用零模型 Token 的本地定时任务：

```bash
tokenchronicle run-daily
tokenchronicle schedule enable --time 03:20 --confirm-background-schedule
tokenchronicle schedule status
```

先观察至少 7 个自然日的状态和日志，再停用旧 Codex Automation。两个调度机制不应长期并行。

程序文件与用户数据彼此分离。升级只替换插件或程序代码，不覆盖归档、配置、反馈草稿和本地分析结果。
