# Fitcore 跨端接口契约登记（待Codex/LHY共同确认）

> 首版取自 `Dnlock/fitcore-core` 历史API交接文档，不代表今日AWS部署已与此完全一致。**每次涉及接口、音频、字段、安全边界的更改，都必须在变更日志中给出新旧差异，审查后再更新本文件。**

| 流程 | 已知接口 / 格式 | 注意事项 | 验收方 |
|---|---|---|---|
| 创建训练Session | `POST /v1/sessions` | 201；保存服务器返回session_id；重复ID可能409 | LHY/Codex |
| 完整WAV上传 | `POST /v1/sessions/{sid}/audio-jobs` | multipart、完整WAV；202只表示持久化接受，不是训练已入库 | LHY/Codex |
| 查询语音Job | `GET /v1/sessions/{sid}/audio-jobs/{uid}` | 轮询直至done/failed；同一uid与sequence重试应保持幂等 | LHY/Codex |
| 增量事件 | `GET /v1/sessions/{sid}/events?after=0&limit=100` | 使用cursor；事实被纠错时不是每条事件都代表新训练组 | LHY/Codex |
| 可信组 | `GET /v1/sessions/{sid}/facts` | Fact Engine COMMIT/DEFER/REJECT；记录与计划严格分离 | Launch/Codex/LHY |
| 待确认 | `GET /v1/sessions/{sid}/pending` | 人工确认操作仅在契约允许且上下文有效时提交 | Launch/Codex/LHY |
| 结束训练 | `POST /v1/sessions/{sid}/finish` + `GET /finish` | 202=draining，409可能缺音频；完成后才读取Review | Codex/LHY |
| 训练总结 | `GET /v1/sessions/{sid}/review` | 未完成Finish的音频Session可能返回409 | Codex/LHY |
| 认证 | `Authorization: Bearer <token>` | 客户端不得持有模型Key；共享Token仅适合授权演示，不支持多用户强隔离 | Codex/LHY |
| 来源校验 | 服务端来源判定 | `server_authorized_test`不代表已做真实声纹验证；不能从客户端自行传VERIFIED_USER | Launch/Codex/LHY |
| 音频要求 | 历史后端接口要求16kHz单声道PCM16 WAV | 实际SDK输出音频格式应由LHY实机核实，转换不应改变原录音证据 | LHY/Codex |

参考源：`Dnlock/fitcore-core/docs/FITCORE_STEP2_LHY_API_HANDOFF_2026-10-15.md`及 `docs/STABLE_BASELINE_WAV_HTTP_REPORT_2026-10-15.md`，应以所引用代码Commit重验契约，不据此自动部署。

## 变更要求

- 修改字段/路径/返回状态时，写明旧行为/新行为、破坏性、请求响应脱敏样例、最低兼容版本、迁移方案、下游ACK名单。
- 两端未ACK时状态设为`CONTRACT_PENDING`，不能宣称“已经联调完成”。
- SDK拿到后，LHY必须补：SDK准确版本、授权方式（不写Token）、音频采样率/封装、真机机型与iOS版本、SDK文件传输机制。