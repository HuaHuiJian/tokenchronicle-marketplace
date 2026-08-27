# TokenChronicle 兼容性说明

## 支持的运行环境

- Python 3.9 至 3.13；
- 本机已安装并使用过 Codex，其只读状态中存在 `state_5.sqlite` 与 rollout 记录；
- 可以访问回环地址的本地浏览器。

发布 CI 在 Linux 的 Python 3.9 与 3.13、当前 GitHub macOS 和 Windows runner 的 Python 3.13
上运行客户端测试。只有某个发布提交的对应工作流真实完成后，矩阵才构成该版本的验收证据。

## 平台功能

| 能力 | macOS | Windows | Linux |
| --- | --- | --- | --- |
| 只读归档、报表、本地浏览器 | 支持 | 支持 | 支持 |
| 零模型 Token 本地调度 | launchd | Task Scheduler | 手工或外部调度器 |
| iCloud 加密快照 | 需要 `hdiutil` 与 iCloud Drive | 不支持 | 不支持 |
| Codex Automation | 可选，在 Codex 中单独管理 | 可选，在 Codex 中单独管理 | 可选，在 Codex 中单独管理 |

全新安装默认不会启用调度、联网反馈、云备份或 Codex Automation。

## 兼容性边界

TokenChronicle 仅以只读方式观察 Codex 本地存储。`state_5.sqlite` 与 rollout 格式属于 Codex
实现细节，未来 Codex 更新可能改变它们。必要结构不存在时，`preflight` 和 `doctor` 必须明确
失败；程序不得尝试修复或改写 Codex 状态。

当前 Python wheel 含可读的专有源代码，只适合获授权的私有分发。公开 marketplace 首发仅支持
通过 Developer ID 签名和 Apple 公证的 macOS arm64 与 Intel 客户端。Windows 需要通过 Microsoft
Store 认证并由 Store 签名后再加入公开发行；未签名 Windows 候选仅用于兼容性测试。
