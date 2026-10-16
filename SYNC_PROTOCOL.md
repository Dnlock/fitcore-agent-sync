# Fitcore Agent 跨仓库同步协议（V1）

## 1. 更新的原子粒度

**一项可验收的实际变化 = 一条独立Markdown。** 例如：一个Bug修复、一个API变更、一个SDK突破、一个评测集更新、一次集成验收，分别记独立条目。若一个Git commit同时跨多个技术界面，可以一个条目记录全部变更，并在“接口/下游影响”逐项说明；不以聊天消息条数代替实际变更。

日志按追加事件维护，发布后**不得静默修改**。发现错误，提交 `CORRECTION` 新条目，填写 `supersedes` 和旧记录文件名。

## 2. 标识与文件名

格式：`updates/YYYY-MM-DD/YYYYMMDD-HHMMSSZ_<AGENT>_<TASK>_<sha7>.md`（UTC；AGENT=`LAUNCH`/`CODEX`/`LHY`/`CHATGPT`）。无代码Commit时用 `NO-CODE`，并标明原因。相同时间撞名时附加 `-a` 等随机短后缀，先查远端，严禁覆盖。

每份报告必须有：`update_id`、`agent`、`task_id`、`type`、`status`、`repo/branch/commit`、`changed_files`、`before_after`、`test_evidence`、`api_or_schema_impact`、`dependencies`、`requires_ack`、`merge_status`、`deploy_status`、`next_actions`。

## 3. 推荐状态枚举

- `LOCAL_ONLY`: 有本地成果、无可验证远端代码Commit。
- `PUSHED_UNREVIEWED`: 远端分支可见、未集成评审。
- `REVIEWED`: 已审查，但不代表已合并/上线。
- `MERGED_NOT_DEPLOYED`: 代码已集成到目标分支，线上仍旧版本。
- `DEPLOYED_VERIFIED`: 有部署Commit及真实环境验证证据。
- `BLOCKED`: 缺少权限、真实数据、SDK或接口依赖。
- `DOC_ONLY`: 仅协议/说明更新，没有代码变动。

## 4. 三方对齐协议

### Launch 改算法 → Codex → LHY

Launch提供输入示例、预期决策、Fact/Pending字段变化、安全回归及SHA；Codex基于准确版本审查接口并决定是否集成；若iOS展示/用户交互变化，LHY按明确接口版本验收并ACK。

### Codex 改后端 API → LHY + Launch

Codex必须提供真实请求/响应和失败状态、兼容性分类、认证/来源边界、迁移步骤；LHY核对自己的iOS代码调用；Launch核对Fact及上下文约束。有任何Breaking change必须由受影响端ACK后才考虑集成。

### LHY 改SDK/iOS → Codex + Launch

LHY写明实机设备型号、SDK/授权状态、音频格式和传输路径、iOS构建或Commit证据、真实/模拟来源的边界；Codex核对后端WAV上传要求；Launch仅在音频与语义输入发生真实变化时参与。

### 项目协调确认

ChatGPT或授权协调者审核变更链接、状态口径与受阻项，更新 `CURRENT_STATE.md`。**聊天消息不能替代这一步GitHub版本化交接。**

## 5. ACK握手

接收Agent需按 `templates/ACK.md` 新增独立文件，引用对方`update_id`以及所读Commit。ACK四种：`ACCEPTED`, `NEEDS_CHANGE`, `BLOCKED`, `INFO_ONLY`。不能只回“收到”，必须说明接口和版本是否兼容、下一步动作及责任人。

## 6. 并发控制

- 默认每个Agent用单独 `agent/launch` / `agent/codex` / `agent/lhy` 日志分支，经PR合入 `main`（按仓库实际权限与审查方式调整）。
- 如果赛事时间限制采用直接向main追加：只能新增唯一文件，写之前fetch/pull，遇到Git冲突先pull/rebase本条日志再push，严禁force push。
- `CURRENT_STATE.md`、`INTERFACE_CONTRACTS.md` 为单写者/审查后更新；避免Agent同时重写快照造成信息丢失。
- 代码与日志位于不同GitHub仓库，不存在原子事务，跨仓库推送成功状态必须分别写明。

## 7. 核验规则

- 所有“已完成”必须有远端代码SHA、测试证据与范围；真实设备/模型/生产环境必须有相应实际证据。
- LHY的隐藏人工评测和真实WAV不可上传本仓库；仅放脱敏汇总、允许分享的报告链接或摘要哈希。
- `fitcore-ios`仓库若为空，不表示LHY没有本地工程；必须由LHY确认并发布可访问的代码版本。
- 发布至生产/主分支必须获得人类授权；日志并不能代替审批。

## 8. 发生冲突时

1. 保留事实，写 `templates/BLOCKER.md`；指出哪份日志、哪个代码SHA、哪条契约冲突。
2. 暂停冲突接口的集成或上线；其他独立任务继续。
3. 由对应代码owner提出修正及验证；下游AGENT重新ACK。
4. 只有新的核验日志能关闭缺陷，禁止覆盖原记录或人工标准。