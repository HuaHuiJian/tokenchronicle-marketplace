# TokenChronicle Architecture Diagrams

本文档使用 TokenChronicle 首发版本的正式名称、域名和协议，作为产品、部署与安全设计的统一图集。

## 1. 总体架构

```mermaid
flowchart LR
    subgraph Client["用户设备"]
        Codex["Codex 本地会话与过程记录"]
        Collector["TokenChronicle 只读采集器"]
        Archive["本地归档、索引与统计"]
        Viewer["TokenChronicle Web 浏览器"]
        Drafts["反馈草稿与回执\n永久本地保留"]
    end

    subgraph Cloud["TokenChronicle 反馈服务"]
        Domain["feedback.tokenchronicle.moreglasses.com.cn"]
        Edge["HTTPS、WAF、限流"]
        Receiver["Feedback Receiver API"]
        Store["持久化反馈存储"]
    end

    Codex -->|"只读观察"| Collector
    Collector --> Archive
    Archive --> Viewer
    Viewer --> Drafts
    Drafts -. "用户逐次确认后发送\n不包含会话内容" .-> Domain
    Domain --> Edge --> Receiver --> Store
```

## 2. 每日自动归档流程

```mermaid
flowchart TD
    Trigger["每日 Automation 触发"] --> Read["只读扫描 Codex 可观察文件"]
    Read --> Parse["解析会话、任务、Automation 与 Token 事件"]
    Parse --> Identity["按 thread id 合并归档片段\n同步当前会话标题"]
    Identity --> Redact["生成脱敏事件与人类可读时间线"]
    Redact --> Files["按项目 / 会话 / 临时文件分层保存"]
    Files --> Usage["刷新每日、项目、会话 Token 统计"]
    Usage --> Memory["生成外部记忆观察与候选摘要"]
    Memory --> Verify{"归档与统计校验通过?"}
    Verify -->|"是"| Publish["更新本地 Web 索引"]
    Verify -->|"否"| Preserve["保留已有归档\n记录失败但不修改 Codex 核心"]
```

## 3. 安装实例激活时序

```mermaid
sequenceDiagram
    autonumber
    actor Operator as "服务管理员"
    participant Admin as "私有管理入口"
    participant Store as "持久化存储"
    actor User as "TokenChronicle 用户"
    participant Client as "TokenChronicle 客户端"
    participant API as "公开反馈 API"

    Operator->>Admin: POST /v1/admin/invitations
    Admin->>Store: 保存一次性邀请码哈希与过期时间
    Admin-->>Operator: 返回 tci1_ 邀请码
    Operator-->>User: 通过可信渠道交付邀请码
    User->>Client: 确认联网并输入邀请码
    Client->>API: POST /v1/installations/activate
    API->>Store: 校验邀请码哈希、有效期和未使用状态
    Store-->>API: 校验通过并原子标记已使用
    API->>Store: 保存 tc1_ 安装凭证的 SHA-256 哈希
    API-->>Client: 凭证明文仅返回一次
    Client->>Client: 保存到私有本地配置
```

## 4. 反馈提交时序

```mermaid
sequenceDiagram
    autonumber
    actor User as "用户"
    participant UI as "TokenChronicle Web UI"
    participant Local as "本地草稿与回执"
    participant Edge as "HTTPS / WAF / 限流"
    participant API as "Feedback Receiver"
    participant Store as "反馈存储"

    User->>UI: 填写产品反馈
    UI->>UI: 排除会话、路径、thread id 与凭证
    UI->>Local: 永久保存待审阅草稿
    UI-->>User: 展示精确 JSON 发送内容
    User->>UI: 逐次确认发送
    UI->>Edge: POST /v1/feedback + tc1_ + Idempotency-Key
    Edge->>Edge: TLS、方法、大小、IP 与流量检查
    Edge->>API: 转发合规请求
    API->>Store: 校验凭证哈希与幂等键
    Store-->>API: 新建记录或返回原回执
    API-->>UI: tcf_ 反馈编号与接收时间
    UI->>Local: 永久保存有限回执
    UI-->>User: 展示提交结果
```

## 5. 阿里云部署架构

```mermaid
flowchart TB
    Client["TokenChronicle 客户端"] -->|"HTTPS 443"| DNS["feedback.tokenchronicle.moreglasses.com.cn"]
    DNS --> Cert["自定义域名与 TLS 证书"]
    Cert --> WAF["阿里云 WAF / 云原生 API 网关"]
    WAF -->|"公开反馈与激活路由"| FC["Function Compute\nNode.js 20 Web Function\n0.0.0.0:9000"]

    Admin["私有管理入口\nMFA / 零信任"] -->|"管理路由"| FC
    FC -->|"RAM 最小权限角色"| OTS["Tablestore"]

    subgraph Tables["TokenChronicle 表"]
        Feedback["tokenchronicle_feedback"]
        Installations["tokenchronicle_installations"]
        Invitations["tokenchronicle_invitations"]
        Idempotency["tokenchronicle_idempotency"]
    end

    OTS --> Feedback
    OTS --> Installations
    OTS --> Invitations
    OTS --> Idempotency

    FC --> Monitor["日志、指标与成本告警"]
    WAF --> Monitor
    OTS --> Backup["加密备份与恢复验证"]
```

## 6. 安全防护与受攻击时的处置链路

```mermaid
flowchart LR
    Traffic["Internet 请求"] --> TLS{"HTTPS 与证书有效?"}
    TLS -->|"否"| Reject1["拒绝"]
    TLS -->|"是"| WAF{"WAF 方法、大小、IP 限制"}
    WAF -->|"异常"| Reject2["阻断并限流"]
    WAF -->|"通过"| Credential{"tc1_ 凭证有效且未撤销?"}
    Credential -->|"否"| Reject3["401 / 429"]
    Credential -->|"是"| Schema{"字段白名单与隐私声明有效?"}
    Schema -->|"否"| Reject4["400 / 413"]
    Schema -->|"是"| Persist["幂等持久化"]

    Detect["异常流量或成本告警"] --> Kill["关闭公开写入开关"]
    Kill --> Revoke["撤销安装凭证并轮换管理员密钥"]
    Revoke --> Inspect["检查指标与存储完整性"]
    Inspect --> Restore["低全局限流下恢复"]
```

## 7. 数据隔离与持久化边界

```mermaid
flowchart TB
    Package["干净程序包"]
    Package --> Code["代码、静态资源、Schema、文档、合成测试"]
    Package -. "绝不包含" .-> Excluded["会话、路径、thread id、设置、日志、凭证"]

    subgraph Local["用户本地 TOKENCHRONICLE_HOME"]
        Config["配置与明确同意状态"]
        Sessions["会话归档与原始证据"]
        Stats["Token 统计与索引"]
        Memory["外部记忆观察结果"]
        FeedbackLocal["反馈草稿与有限回执\n默认永久保留"]
    end

    subgraph Server["服务端持久化边界"]
        FeedbackServer["用户主动提交的反馈文本"]
        CredentialHash["安装凭证哈希"]
        IdempotencyServer["幂等键与有限回执字段"]
    end

    Sessions -. "不会自动上传" .-> Server
    FeedbackLocal -->|"仅逐次确认的隐私边界内载荷"| FeedbackServer
    FeedbackServer --> Retention["RETENTION_DAYS=0\n默认永久保留"]
```
