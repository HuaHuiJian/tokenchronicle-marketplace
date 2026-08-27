# TokenChronicle 词元日志升级指南

程序文件与用户数据相互分离。升级时应替换程序目录，不覆盖用户的配置、归档、反馈草稿和实例凭证目录。

## 推荐步骤

1. 保留当前用户数据目录，必要时先做文件系统备份。
2. 解压新的干净发布包到新目录。
3. 运行 `tokenchronicle preflight` 检查环境与只读数据源。
4. 运行 `tokenchronicle doctor` 验证配置迁移、目录权限和服务端口。
5. 启动新版本并检查项目数量、会话数量和历史报表，再停止旧版本。
6. 如果此前启用了本地系统调度，应从新运行时重新执行
   `tokenchronicle schedule enable --time HH:MM --confirm-background-schedule`，使 launchd 或
   Windows Task Scheduler 明确指向新版本；完成一次手工 `run-daily` 和调度状态核验前不要删除旧运行时。

配置使用带版本号的 schema 自动迁移。升级不得自动启用 Automation、联网反馈或原始事件复制。机器接口、CLI 命令、配置键和数据目录继续使用稳定的英文标识，以保障跨语言升级兼容性。

如需回退，停止新版本并重新启动旧程序目录；不要删除或回写用户数据目录。
升级不会创建、移动或删除用户的 iCloud 加密快照。
