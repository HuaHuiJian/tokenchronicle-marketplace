# TokenChronicle Token 使用说明

## 默认消耗

全新安装不会自动创建 Codex Automation，因此默认每日模型 Token 消耗为 **0**。浏览本地报表、搜索归档和切换语言不会调用模型。

## 启用每日 Automation 后

以每日运行一次为规划口径，预计每次约 **0.7M-2.0M total tokens**，常见约 **1.0M-1.5M**；30 日约 **21M-60M**。首次全量扫描或历史会话较多时可能高于日常增量运行。这些范围来自匿名化的发布前容量基准；程序与发行包不包含该基准的原始会话、账户、路径、逐日记录或用户设置。

这里的 `total_tokens` 包含缓存输入，只用于容量规划，不等于账单金额。实际数值会受到模型、上下文窗口、缓存命中、归档规模和工具输出影响。

## 用户选择权

- 保持 Automation 关闭：每日模型 Token 为 0，需要时手动归档。
- 启用每日 Codex Automation：自动整理并消耗上述估算范围内的模型 Token。
- 使用操作系统本地定时任务：程序内置 macOS launchd 与 Windows Task Scheduler 适配器，但默认
  关闭，只有用户明确执行 `schedule enable --confirm-background-schedule` 后才注册。

可运行 `tokenchronicle usage-notice` 查看当前版本的机器可读估算参数。
