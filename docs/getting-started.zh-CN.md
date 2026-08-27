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

安装流程会分别展示插件程序、运行状态和长期档案库。Codex 插件优先使用官方提供的
`PLUGIN_DATA` 保存配置、锁和日志；长期档案库由用户选择。macOS 独立运行时的默认数据目录为：

```text
~/Library/Application Support/TokenChronicle/
```

用户不需要手工创建。长期档案库可以使用默认私有位置，也可以精确选择 Documents、iCloud Drive
或其他目录：

```bash
tokenchronicle setup --accept-privacy \
  --codex-home /path/to/.codex \
  --archive-dir ~/Documents/TokenChronicle \
  --language zh-CN \
  --port 8877
```

如果 Codex 或 macOS 要求访问所选目录，安装流程必须先展示准确路径并征得用户授权。用户拒绝后，
TokenChronicle 不会绕过决定，可以改选其他可写目录并重新预检。

## 可选功能和成本

- 手工归档不会引入模型 Token 消耗。
- 所有调度默认关闭，产品默认后台消耗为 `0 Token/日`。
- 首次归档成功后可以单独启用本地操作系统定时任务；该确定性任务不使用模型 Token。
- 显式启用每日 Automation 后，建议按每次 `0.7M-2.0M total_tokens` 规划。
- 反馈默认关闭，每次联网发送都必须单独确认。
- 默认不复制未脱敏的原始 rollout 文件。

完整估算口径见 `docs/token-usage.md`。

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

程序文件与用户数据彼此分离。升级只替换插件或程序代码，不覆盖归档、配置、反馈草稿和本地分析结果。
