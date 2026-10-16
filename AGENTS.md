# AGENTS.md — Fitcore 全体 Agent 工作契约

本文件面向能读取本GitHub仓库的 Launch、Codex、LHY Agent、ChatGPT。**读取本文件即意味着必须遵守交接格式；但是要求本身不能保证你的工具自动执行，实际需要Agent拥有GitHub权限。**

## 角色

- **Launch：** 原TRAE的A01–A06、F01–F08、TC-01–TC-06、数据质量/评测、算法修复；不改生产环境或LHY独立Ground Truth。
- **Codex：** C01–C07、FastAPI/WAV/Job/Fact、接口与AWS、受控集成和Git资产保护；未经授权不得合并/部署。
- **LHY Agent：** soundcore SDK、iOS客户端、真实录音接入、脱敏真机验证、LHY人工评测数据的权属管理；不擅自更改服务端判定或部署。
- **ChatGPT：** 路线与交接审核、更新快照/契约的审核建议、辅助核验；非代码仓库实际变更不得声称已落地。

## 每次任务的强制生命周期

`SYNC_READ -> SCOPE -> IMPLEMENT -> TEST -> CODE_PUSH -> CHANGELOG_PUSH -> RECEIVER_ACK -> (经授权的MERGE/DEPLOY)`

任一步缺证据必须明确写 `BLOCKED` / `UNKNOWN`；不能靠聊天中的“完成了”推断已部署。

### 开始前

- 拉取最新远端；读 `README.md`、`CURRENT_STATE.md`、`INTERFACE_CONTRACTS.md` 与最近10条更新。
- 识别所有依赖任务及历史阻塞，声明本次任务ID、所属代码仓库、工作分支、预期影响Agent。
- 远端仓库尚未同步时标注 `SYNC_STALE`，不要瞎编最新状态。

### 工作完成后

- 提供准确的变更文件列表、输入输出行为变化、接口Diff、重要安全边界。
- 测试用例写清**执行命令、环境、通过/失败/跳过、真实或合成证据**；不能把不同工作树测试数累加。
- 记录任务是否已push、merge、deploy，给出链接与SHA。不上线不得写“线上已修复”。
- `updates/YYYY-MM-DD/YYYYMMDD-HHMMSSZ_<AGENT>_<TASK>_<sha7>.md` 只新增，不能覆盖。
- 写入时需检查同名文件已不存在；网络失败时保留本地日志并标注 `LOCAL_ONLY`。
- 列出 `requires_ack`，明确下游“必须修改/需要验证/仅知会”；接收者使用单独 ACK 文件。

### 安全

- 不提交 `.env`、Token、密钥、`.pem`、真人WAV、原始转写、身份证明、真实数据库、人工隐藏评测答案。
- 保留原始Ground Truth与已授权数据，不能改写“使测试通过”。
- 不跨Worktree强制checkout/reset、不得擅自force push、merge或部署AWS。
- 生产 `VERIFIED_USER` 与授权单人演示来源 `server_authorized_test` 不是一回事。

## 同步失败策略

**代码推送成功、日志推送失败 ≠ 完成交接。** 保留目标提交SHA、待发布日志文件路径和失败原因，通知相关Agent等待。**日志有记录、代码未推送 ≠ 功能可用。** 日志状态必须为 `LOCAL_ONLY`。