# AGENTS.md — Fitcore 全体研发工作契约

本文件面向能读取本GitHub仓库的 Launch、Codex、LHY 与 ChatGPT。**读取本文件即意味着必须遵守交接格式；但是要求本身不能保证工具自动执行，实际需要对应 GitHub / AWS / 本地设备权限。**

## 角色

- **LHY（真人 Project Owner / Developer）：** 不是 Agent。负责项目决策、真机与录音豆物理操作、SDK/License/Token 等私密配置、AWS/SSM 人工操作、人工 Review 与最终 merge/deploy/Gate 批准；不得把真实音频、隐藏 Ground Truth 或密钥提交公共同步仓库。
- **Launch：** 负责 iOS 客户端、soundcore SDK 接入、音频采集/标准化、客户端 HTTP 适配、UI，以及全部算法研发（含原 TRAE 的 Track A / B / C）。不部署 AWS，不读取 LHY 私密凭据，不修改隐藏人工 Ground Truth。
- **Codex：** 负责 FastAPI/WAV/Job/Fact、HTTP Contract、AWS、部署、系统集成与工程验收；未经 LHY 明确批准不得 merge、切公网、执行受控 Gate 之外的 AWS 变更。
- **ChatGPT：** 架构协调、状态核验、跨 Agent 任务管理、同步快照与交接审核；不把聊天口头状态冒充代码/设备/部署证据。
- **TRAE：** `ARCHIVED / DO_NOT_REACTIVATE`。历史分支、任务号和证据可保留引用，但不得重新分配新工作。

## 每次任务的强制生命周期

`SYNC_READ -> SCOPE -> IMPLEMENT -> TEST -> CODE_PUSH -> CHANGELOG_PUSH -> RECEIVER_ACK -> (经授权的MERGE/DEPLOY)`

任一步缺证据必须明确写 `BLOCKED` / `UNKNOWN`；不能靠聊天中的“完成了”推断已部署。

### 开始前

- 拉取最新远端；读 `README.md`、`CURRENT_STATE.md`、`INTERFACE_CONTRACTS.md` 与最近10条更新。
- 识别所有依赖任务及历史阻塞，声明本次任务ID、所属代码仓库、工作分支、预期影响对象。
- 远端仓库尚未同步时标注 `SYNC_STALE`，不要瞎编最新状态。

### 工作完成后

- 提供准确的变更文件列表、输入输出行为变化、接口Diff、重要安全边界。
- 测试用例写清**执行命令、环境、通过/失败/跳过、真实或合成证据**；不能把不同工作树测试数累加。
- 记录任务是否已push、merge、deploy，给出链接与SHA。不上线不得写“线上已修复”。
- `updates/YYYY-MM-DD/YYYYMMDD-HHMMSSZ_<AGENT>_<TASK>_<sha7>.md` 只新增，不能覆盖。
- 写入时需检查同名文件已不存在；网络失败时保留本地日志并标注 `LOCAL_ONLY`。
- 列出 `requires_ack`，明确下游“必须修改/需要验证/仅知会”；接收者使用单独 ACK 文件。

### 安全

- 不提交 `.env`、Token、密钥、`.pem`、真人WAV、原始转写、身份证明、真实数据库、人工隐藏评测答案、未获授权的私有 SDK binary。
- 保留原始 Ground Truth 与已授权数据，不能改写“使测试通过”。
- 不跨 Worktree 强制 checkout/reset、不得擅自 force push、merge 或部署 AWS。
- 生产 `VERIFIED_USER` 与授权单人演示来源 `server_authorized_test` 不是一回事。
- `fitcore-agent-sync` 为公开仓库，只允许脱敏状态、版本、指标和非敏感交接摘要。

## 同步失败策略

**代码推送成功、日志推送失败 ≠ 完成交接。** 保留目标提交SHA、待发布日志文件路径和失败原因，通知相关接收方等待。**日志有记录、代码未推送 ≠ 功能可用。** 日志状态必须为 `LOCAL_ONLY`。
