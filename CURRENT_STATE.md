# Fitcore 当前项目状态（实时协调快照）

**as_of：2026-10-16T10:15Z**。本文件由协调者基于 GitHub 远端、LHY 真机/AWS人工操作反馈及 Launch/Codex 当前交接更新。聊天消息本身不等于部署或真机验证；以下状态按证据边界标注。

## 1. 角色与仓库

| 方向 | 当前责任 | 真实状态 |
|---|---|---|
| 项目 Owner / 人工操作 | **LHY（真人，不是 Agent）** | 负责真机、SSM、私密凭据、最终审批与人工 Review |
| iOS / soundcore / 客户端 / 算法 | **Launch** | 正在迁移 canonical iOS、准备 soundcore SDK 审计与合同适配 |
| FastAPI / HTTP / AWS / 部署 / 集成验收 | **Codex** | IB-03 Gate 1 仍阻塞于 SSM source transfer 未完成 |
| 架构协调 / 状态核验 | **ChatGPT** | 负责 GitHub 同步、跨 Agent 任务拆解与 Gate 口径 |
| TRAE | **ARCHIVED** | `DO_NOT_REACTIVATE`；仅保留历史证据引用 |

Canonical repos：

- `Dnlock/fitcore-agent-sync`：公开跨 Agent 状态/接口/ACK 同步仓库。
- `Dnlock/fitcore-ios`：Launch 客户端 canonical repo。
- `Dnlock/fitcore-core`：Codex backend / Core canonical repo。
- `Dnlock/fitcore-coordination`：私有项目控制与历史协调资料；不替代本公开同步仓库。

## 2. Launch / iOS 当前状态

### MIG-01 canonical migration

- branch：`launch/ios-migration-20261016`
- remote HEAD：`17c60dca0fa2fa49a11837807f8f5435f63d971c`
- base `main`：`4f27908f25d08687e36be39347e56e5f9e5ff9a7`
- GitHub compare：migration branch 相对 main **ahead 3 / behind 0**，已实际包含完整 iOS 工程、测试和迁移文档。
- migration source provenance：历史源 `a98d47003f21cef87169333898c691f57fe60f29`。
- Draft PR：`Dnlock/fitcore-ios#2`，已创建，**未 merge**。

### 已验证 / 已实现

- 迁移内容：App shell、DesignSystem、Core Models、WAVCapture、历史 WAVHTTP client、CloudTrainingModel、MockCloudBackend、Today/Training/Capture/Review、tests、工程脚本。
- 安全扫描：Launch 报告 CLEAN；排除 WAV / PII / credentials / Local.xcconfig / private SDK binary / DerivedData。
- Linux Swift tests：**74/74 passed, exit 0**。
- 历史 UI evidence：17 张 Simulator 截图，明确 `MOCK/SIMULATOR`、`REAL_HTTP=false`。
- LC-D：`SATISFIED`（branch created + pushed）。
- LC-E：`SATISFIED`（security scan clean）。
- LC-F：`PARTIAL`（Linux tests 绿；macOS `verify.py` / `xcodebuild` 尚未执行）。
- LC-G：`WAITING_FOR_IB03`。

### 未完成 / 阻塞

- 新合同 RED-01..09 已规格化，但**尚未实际编写运行**；等待 Codex 对 A02/A03 / Pending resolve 等 frozen mapping 确认。
- `Pending resolve` 仍为 P0 UI/contract gap。
- Real HTTP 未验收。
- 物理 iPhone 主链未验收。
- soundcore SDK 仅完成 adapter plan；SDK 审计、授权验证、真实设备音频读取、格式确认与 adapter 实现尚未完成。
- 当前不能把官方 soundcore App 成功、Simulator Mock 或 backend 单独成功写成 Fitcore 真机闭环成功。

## 3. Codex / IB-03 当前状态

唯一部署基线：

- HTTP contract：`fitcore-http-v1.1-20261016`
- pinned Core：`479748526bf731b2dfe9292201394019ed7e088c`
- 该 Core SHA 在 `Dnlock/fitcore-core` 可访问并已核实存在。

### Gate 1

- 状态：**`GATE1_BLOCKED`**。
- 原因：目标 EC2 尚未完成 `GATE1_SSM_SOURCE_TRANSFER_2026-10-16.md` 的 10 段 source staging + final verification，尚无 `STAGED_TRANSFER_PASS`。
- 首次 SSM 粘贴尝试没有得到 `PART_VERIFIED`，出现 shell/heredoc 执行错误；因此不能视为 source transfer 成功。
- 目标 release：`/opt/fitcore-ib03/releases/479748526bf731b2dfe9292201394019ed7e088c`
- 目标 service：`fitcore-ib03.service`
- 目标 local port：`127.0.0.1:18080`
- 目标 isolated DB：`/var/lib/fitcore-ib03/fitcore.db`

### Runtime precheck 已知证据

- Python：3.12.3
- `/usr/bin/python3.12`
- `venv=true`
- `ensurepip=true`
- `python3.11=NOT_FOUND`
- Codex 报告已有 Python 3.12 的 850 项离线测试证据；Ubuntu 目标机部署验证尚未完成。

### Gate 1 尚未发生

- `fitcore-ib03.service`：NOT_RUN
- new PID/cwd/module origin/18080 listener：NOT_RUN
- isolated DB 实际创建与写权限：NOT_RUN
- new HTTP token/model key presence：NOT_VERIFIED
- local health / negative auth / positive auth：NOT_RUN
- Caddy：本轮未修改
- old `fitcore.service` / old 8000 / old DB：本轮没有部署变更证据；预检阶段曾为 healthy，但本轮尚未重新验证。

**Gate 2 / Gate 3 均保持锁定。** 未授权 Caddy cutover、F1/F2、真实音频、ASR/LLM 调用。

## 4. Client ↔ Backend 合同状态

客户端目前仍需 adaptation，不能声明与 pinned Core 完全兼容。当前明确关注：

- `session_id` max 120
- AudioJob statuses：`queued / asr_running / final_ready / llm_running / retry_asr / retry_llm / blocked / done`
- Outcome：`COMMIT / DEFER / REJECT / ERROR / DUPLICATE / STALE_CONTEXT / ID_CONFLICT`
- Pending resolve frozen request body / 409 行为
- `/facts` 是 authoritative current snapshot
- `/events` 仅 notification/audit，不能按 event count 推导完成组
- Finish：202 draining / 409 awaiting_audio / same finish_id retry
- Review：server authoritative
- RIR/RPE numeric DTO：当前 contract 未正式提供，不得由客户端自行发明字段

## 5. soundcore 与真实闭环

LHY 已持有真实录音豆硬件与 soundcore SDK；但当前正式状态仍是：

`soundcore录音豆 → Fitcore iOS → 完整WAV → AWS API → ASR → Fact Engine → Facts → Finish → Review`

**NOT YET VERIFIED END-TO-END**。

下一硬件里程碑应先验证：

`soundcore设备 → 官方SDK → Fitcore iOS → 真实音频 bytes/file → source format metadata`

然后才能判断是否需要 decode / resample / downmix / PCM16 encode / WAV packaging。

## 6. 数据安全与评测

- 本仓库是公开仓库。
- 禁止提交真实用户 WAV、未授权 transcript、隐藏人工 GT、SDK License、Token/API Key、AWS 密钥、SSH 私钥、PII、私有 SDK binary。
- 独立人工评测仍未完成；任何开发集、Mock、合成音频或模型已见样本不得冒充独立盲测。

## 7. 当前最高优先级

1. **LHY / Codex：** 修正 SSM source-transfer 执行方式并拿到真实 `STAGED_TRANSFER_PASS`；否则 Gate 1 不继续。
2. **Launch：** 基于 `fitcore-ios#2` 继续 contract RED tests + macOS/Xcode verification，保持 Draft / 不 merge。
3. **Launch + LHY：** 执行 LD-01 soundcore SDK audit，随后由 LHY 真人做第一轮物理设备 smoke test。
4. **Codex：** source transfer PASS 后继续 isolated Gate 1；成功后只能进入 `GATE2_APPROVAL_REQUIRED`，不得自动 cutover。
5. **三方汇合 Gate：** 只有当 Launch 有合法真实设备 WAV、Codex 有 pinned Core 的 isolated 18080 backend、合同 RED/GREEN 通过后，才考虑 Real HTTP Gate 2。
