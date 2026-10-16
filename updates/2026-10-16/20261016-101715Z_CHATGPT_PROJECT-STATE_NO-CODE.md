# PROJECT-STATE Launch/Codex 最新状态同步

```yaml
update_id: "20261016-101715Z_CHATGPT_PROJECT-STATE_NO-CODE"
date_utc: "2026-10-16T10:17:15Z"
agent: "CHATGPT"
task_id: "PROJECT-STATE"
type: "DOC"
status: "DOC_ONLY"
code_repo: "N/A"
code_branch: "N/A"
code_commit: "N/A"
code_commit_url: "N/A"
code_push_verified: false
merge_status: "N/A"
deploy_status: "NOT_DEPLOYED"
affected_agents: ["LAUNCH", "CODEX", "LHY"]
requires_ack: ["LAUNCH", "CODEX"]
supersedes: null
```

## 1. 本次目的和结果

- **解决的问题：** 将 Launch 最新 canonical iOS migration、Codex IB-03 Gate 1 实际阻塞、LHY 真人角色与当前三方汇合条件同步到统一仓库。
- **是否已完成：** 已完成协调快照；未进行代码合并、AWS部署、Caddy切换或真实音频调用。
- **本次不包含：** 不包含 SDK secret、真人音频、隐藏 Ground Truth、AWS credential、生产数据库或 private SDK binary。

## 2. 具体变更（可审计粒度）

| 修改路径 | 之前行为 | 现在行为 | 为何修改 |
|---|---|---|---|
| `AGENTS.md` | 将 LHY 错写为 Agent；Launch职责偏算法 | 明确 LHY 为真人 Owner/Developer；Launch= iOS+soundcore+client+algorithm；Codex=backend+AWS；TRAE archived | 与当前真实组织方式一致 |
| `CURRENT_STATE.md` | 初始快照仍写 `fitcore-ios` 为空、IB-03 状态过时 | 更新 Launch migration、Draft PR、测试边界、Codex Gate1 blocker、合同与硬件下一步 | 让跨 Agent 读取同一真实状态 |
| 本更新文件 | 无 | 新增 append-only 协调事件 | 遵循 SYNC_PROTOCOL |

## 3. 接口、数据与上下游影响

- HTTP 合同本次没有改动；继续以 `fitcore-http-v1.1-20261016` + Core `479748526bf731b2dfe9292201394019ed7e088c` 为当前唯一集成基线。
- Client 当前仍需适配：session_id max120、8种 AudioJob status、7种 Outcome、Pending resolve、Facts authoritative、Finish/Review 等。
- **Codex要做：** ACK 当前 pinned baseline 与 Launch RED mapping；完成 SSM source transfer 后才继续 isolated Gate 1。
- **LHY要做：** 真人执行 SSM / 真机 / 私密 SDK credential 配置与 Gate 批准。
- **Launch要做：** 在 Draft PR `Dnlock/fitcore-ios#2` 上继续合同 RED/GREEN、macOS/Xcode verification、LD-01 SDK audit；不得 merge main。

## 4. 测试和证据

| 命令/场景 | 环境与测试数据来源 | 结果 | 证据链接/脱敏路径 |
|---|---|---|---|
| `fitcore-ios main...launch/ios-migration-20261016` GitHub compare | GitHub remote | branch ahead 3 / behind 0 | `Dnlock/fitcore-ios` |
| Launch migrated Swift tests | Linux / Mock & unit tests | 74/74 passed（Launch报告；GitHub迁移文档可见） | commit `17c60dca0fa2fa49a11837807f8f5435f63d971c` |
| iOS canonical migration PR | GitHub | Draft PR created, not merged | `https://github.com/Dnlock/fitcore-ios/pull/2` |
| Core pinned SHA existence | GitHub | verified accessible | `https://github.com/Dnlock/fitcore-core/commit/479748526bf731b2dfe9292201394019ed7e088c` |
| IB-03 source transfer | EC2 SSM | **not passed**；尚无 `STAGED_TRANSFER_PASS` | LHY/Codex 脱敏反馈 |

- **未覆盖风险/已知失败：** macOS `verify.py` / `xcodebuild` 未在 canonical branch 执行；Soundcore SDK/物理设备链未验证；Real HTTP 未验证；Gate1 backend service 未创建。
- **不得将独立测试视为跨模块集成或真实准确率。**

## 5. 合并/部署状态

- Launch iOS migration HEAD：`17c60dca0fa2fa49a11837807f8f5435f63d971c`
- iOS Draft PR：`https://github.com/Dnlock/fitcore-ios/pull/2`
- iOS merge：NO
- Core deploy baseline：`479748526bf731b2dfe9292201394019ed7e088c`
- IB-03 Gate1：`GATE1_BLOCKED`
- 新 backend deploy：NO
- Caddy cutover：NO
- Gate2 / Gate3：LOCKED

## 6. 下一步 / ACK

- 接收Agent：Launch、Codex。
- **Launch ACK 必须确认：** `17c60dc` 是否仍为当前 migration HEAD；74/74测试范围；RED-01..09尚未运行；Soundcore仍仅 plan / 未真机。
- **Codex ACK 必须确认：** pinned SHA、A02/A03/Pending resolve frozen mapping、`GATE1_BLOCKED` 与 source-transfer-only 下一步。
- 阻塞条件和解法：先拿到真实 `STAGED_TRANSFER_PASS`；客户端合同先 RED 后 GREEN；硬件先 SDK audit 再物理 smoke test。
- 预计下一步：Launch 合同适配与 SDK audit、Codex isolated Gate1、LHY 执行受控人工操作。

## 7. 安全与数据来源

- 是否包含真实录音/隐藏人工GT/密钥/用户个人数据：**NO**。
- 数据来源：GitHub remote refs + Launch 本轮交接 + Codex/LHY 当前 IB-03 脱敏状态。
- 备注：本更新是协调记录，不代表代码已 merge 或 AWS 已部署。
