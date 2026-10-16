# Fitcore Agent Sync — 跨 Agent 研发同步中心

> 状态：初始化待发布（发布完成后以 GitHub 记录为准）。  
> 仓库类型：**PUBLIC**（用户已知悉并允许公开）；仓库：`Dnlock/fitcore-agent-sync`。  
> 更新时间基线：2026-10-16。  
> 管理对象：**Launch（算法）/ Codex（后端集成）/ LHY Agent（iOS与设备）/ ChatGPT（项目协调）**。

## 一句话规则

**凡是实际代码、接口、配置、测试、数据规范发生变化，必须在本仓库追加一份 Markdown 变更记录，引用已经推送成功的目标代码仓库、分支和精确 Commit SHA。未推送则只能标记 `LOCAL_ONLY`，禁止声称其他 Agent 能读取。**

本仓库仅存放变更日志、接口约定、验收证据索引、交接确认、决策，不存放代码库备份、录音、真实训练数据、账号密码、Token或私钥。

> **公开仓库安全边界：** 任何人都可能阅读这里的内容，禁止提交未公开的SDK授权、业务敏感数据、生产主机访问资料、真实音频/转写、个人信息、内部商业报价或未脱敏请求响应。详细错误堆栈、敏感架构和安全漏洞先通过获授权的私有渠道共享，在本仓库只发布经过脱敏的摘要。

## Agent 开始工作前必须读取（按顺序）

1. [`AGENTS.md`](AGENTS.md)：权限、职责、读写规则。
2. [`CURRENT_STATE.md`](CURRENT_STATE.md)：唯一经人工审阅的当前状态快照（有 `as_of` 与核验状态）。
3. [`INTERFACE_CONTRACTS.md`](INTERFACE_CONTRACTS.md)：跨 Agent API、文件格式和数据口径。
4. [`updates/`](updates/)：最近至少 10 条**所有 Agent**的更新记录，以及与当前任务相关的全部未解决更新。按文件名时间和实际Git提交时间交叉核查。
5. [`acks/`](acks/)：自己需要确认的交接与未解决阻塞。

**不得只看README或上一次对话，就假定其覆盖最新的代码状态。** 代码实际内容和远端Commit优先于摘要；矛盾须记入阻塞记录。

## 完成工作前必须做

1. 实施修改 → 运行/记录测试与检查 → 推送到所属代码仓库正确分支。
2. 取得真实代码 Commit SHA 与URL，区分 `NOT_MERGED`、`MERGED`、`DEPLOYED`；只有证据才能标记。
3. 按 [`templates/CHANGE.md`](templates/CHANGE.md) 新增 `updates/YYYY-MM-DD/....md`。不得修改其他 Agent 的日志。
4. 推送本仓库的新增Markdown文件，核对GitHub网页已可读取。
5. 在对话/交接结果中提供**代码Commit链接 + 本仓库变更记录链接**。缺任一项，视为“未完成远端交接”。
6. 指定受影响Agent；接收方读完后，按 [`templates/ACK.md`](templates/ACK.md) 添加ACK或阻塞说明。

## 目录和权责

| 路径 | 内容 | 写入规则 |
|---|---|---|
| `updates/YYYY-MM-DD/*.md` | 细颗粒度、只追加的任务变更日志 | 每个Agent自行新增唯一文件 |
| `acks/YYYY-MM-DD/*.md` | 接收人基于精确更新ID的确认或拒收 | 接收Agent只追加 |
| `decisions/*.md` | 重大接口、安全、优先级和发布决策 | 由用户明确授权/协调后记录 |
| `CURRENT_STATE.md` | 经过验证的项目总体状态 | **只有协调负责人维护**，其他Agent提更新建议 |
| `INTERFACE_CONTRACTS.md` | 经过核准的跨端契约 | **只有Codex/协调负责人在核对后维护** |
| `templates/` | 标准格式 | 修改要另写更新日志 |

## 权威来源与避免双重真相

- `Dnlock/fitcore-core`：代码、测试和算法分支是代码状态权威来源。
- `Dnlock/fitcore-ios`：未来上传的iOS客户端代码权威来源；目前仓库是否有代码以实时核查为准。
- `Dnlock/fitcore-coordination`：原有Codex C01–C07任务与工程交接文档，保留，不迁移/覆盖历史。
- **本仓库**：跨Agent的变更事件、交接和ACK权威来源，不取代目标代码仓库。
- `Launch` 接管原 `TRAE` 的所有 Track A/B/C 工作；保留历史 `trae/` 分支名，**不要擅自重命名**。

详细协议：[`SYNC_PROTOCOL.md`](SYNC_PROTOCOL.md)。