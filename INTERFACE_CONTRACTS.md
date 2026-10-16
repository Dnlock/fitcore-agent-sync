# Fitcore 跨端接口契约登记

> 当前唯一客户端/后端集成基线：**`fitcore-http-v1.1-20261016`**；Pinned Core：**`479748526bf731b2dfe9292201394019ed7e088c`**。本文记录的是集成合同，不代表 AWS 已部署该 SHA。实际线上版本仍须由 Codex 通过 IB-03 attestation 验证。

| 流程 | 当前冻结接口 / 格式 | 客户端必须遵守 | 验收方 |
|---|---|---|---|
| 创建训练 Session | `POST /v1/sessions` | `session_id` 最大 120；保存服务器返回上下文版本；冲突状态不得静默重建身份 | Launch/Codex |
| 完整 WAV 上传 | `POST /v1/sessions/{sid}/audio-jobs` | multipart 完整 WAV；**202 只代表 persisted/accepted，不代表 ASR 成功或 Fact 已提交** | Launch/Codex |
| 查询语音 Job | `GET /v1/sessions/{sid}/audio-jobs/{uid}` | Job status：`queued / asr_running / final_ready / llm_running / retry_asr / retry_llm / blocked / done`；同一逻辑录音重试保持同一 uid/sequence/bytes | Launch/Codex |
| 语义 Outcome | Job outcome | `COMMIT / DEFER / REJECT / ERROR / DUPLICATE / STALE_CONTEXT / ID_CONFLICT`；`done` 本身不等于 `COMMIT` | Launch/Codex |
| 增量事件 | `GET /v1/sessions/{sid}/events?after=<cursor>&limit<=100` | Events 是 notification/audit envelope；不能按 event 数量推导完成训练组；cursor 冲突需显式处理 | Launch/Codex |
| 可信 Facts | `GET /v1/sessions/{sid}/facts` | **authoritative current snapshot**；纠错后以最新 snapshot 替换，不从本地事件累计创造事实 | Launch/Codex |
| Pending | `GET /v1/sessions/{sid}/pending` | 只展示当前 open pending；decode/refresh 失败不得伪装成“没有待确认内容” | Launch/Codex |
| Pending resolve | `POST /v1/sessions/{sid}/pending/resolve`（按当前 Core 实际路径/handler核验） | 冻结请求核心字段：`resolution_id`、`action`（`dismiss` / `confirm_set`）、`context_version`；409/STALE 必须显式 surfaced；未冻结字段不得由客户端自行发明 | Launch/Codex |
| 结束训练 | `POST /v1/sessions/{sid}/finish` + `GET /finish` | 202 = draining；409 可能 `awaiting_audio`；retry 必须复用同一 `finish_id`；缺音频补传后再继续 | Launch/Codex |
| Review | `GET /v1/sessions/{sid}/review` | server authoritative；未 ready/finish 时的 409 不得伪造本地 review | Launch/Codex |
| 认证 | `Authorization: Bearer <HTTP token>` | 客户端只持有 HTTP token；**MODEL_API_KEY 永远 server-only**；secret 不进入 Git/日志/同步仓库 | Codex/LHY |
| 来源校验 | 服务端来源判定 | `server_authorized_test` ≠ 生产 `VERIFIED_USER`；客户端不得自行声明 verified identity | Launch/Codex/LHY |
| 音频要求 | 完整 RIFF/WAVE、PCM16 little-endian、16kHz、mono、non-empty、`<= 7,000,000` bytes | soundcore source format 先由真实 SDK/设备核实；如需转换，应保留 source audio 与 normalized upload WAV 的区分 | Launch/Codex/LHY |
| RIR/RPE | 当前 V1.1 无正式 numeric DTO | 客户端不得自行创造 numeric contract；主观表达由现有语义安全规则处理 | Launch/Codex |

## 当前客户端适配状态

- `Dnlock/fitcore-ios` migration branch 已迁入历史客户端，但仍需 RED/GREEN 适配上述 120 / 8 statuses / 7 outcomes / Pending resolve / correction 等合同点。
- Facts authoritative、202≠Fact、Finish draining/409、Review server-source 等历史原则已经存在，但不能代替对 pinned Core 的真实兼容测试。
- Real HTTP integration 仍等待 IB-03 Gate 1 + 后续独立 Gate 2 批准。

## 变更要求

- 修改字段/路径/返回状态时，必须写明旧行为/新行为、breaking 分类、脱敏请求响应、最低兼容版本、迁移方案与 `requires_ack`。
- Launch 与 Codex 未 ACK 的 breaking change 状态必须保持 `CONTRACT_PENDING`，不能宣称“已联调完成”。
- soundcore SDK audit 后补充：SDK准确版本、授权方式（不写 secret）、真实传输机制、source codec/container/sample rate/channels/bit depth，以及是否需要 normalize。
