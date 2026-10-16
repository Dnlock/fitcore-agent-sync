# Launch P0 — Contract / Soundcore / Algorithm / Device Readiness（2026-10-16）

## 摘要

- 角色版本：LHY=真人Owner；Launch=iOS客户端+soundcore+HTTP适配+音频规范化+算法+测试；Codex=backend/HTTP guardian/AWS/集成；ChatGPT=协调；Trae=ARCHIVED
- 本轮：RED-01..09 → 最小 GREEN（95/95 Linux PASS）；Mac/Xcode/LHY 操作单；SDK 审计状态；算法矩阵；引用审计

## CONTRACT

- 依据 pinned Core `479748526bf731b2dfe9292201394019ed7e088c` 真实 schema（非口头确认）
- RED tests 10 条 → 最小 production 修复（WAVHTTPModels enum 扩展 + PendingResolution 字段 + 支撑类型）
- branch `launch/contract-v11-red-green` @ `38ad0554aa222a4d73be83b2f56ac3496d8538da`（stacked on 17c60dc，不污染 migration provenance）
- 报告：`docs/contract/CLIENT_CONTRACT_V11_REPORT.md`
- 剩余缺口：RED-06 独立 test 未写；unknown enum UI surfaced 未做；真实 HTTP 未联调（IB-03）

## MAC / XCODE / DEVICE

- 生成 LHY 操作单（真实 scheme=Fitcore-Mock、project=ios/Fitcore.xcodeproj、bundle=com.example.fitcore）
- LHY ACTION REQUIRED：fetch → checkout 38ad0554 → verify.py → xcodebuild → simulator smoke → physical iPhone
- Linux 95/95 ≠ Xcode PASS；Simulator ≠ 真机；未回传前不写 PASS

## SOUNDCORE

- SDK 状态：SOUNDCORE_SDK_CREDENTIALS=AVAILABLE_OUT_OF_BAND
- LD-01 审计：SDK_PACKAGE_REQUIRED（私有包未提供 → 不凭文档猜 API）
- transport 机制：未确认（A~G 七选一待 SDK 到位后判定）

## AUDIO / ALGORITHM

- 规范化管线（SOURCE→inspect→decode→resample→downmix→PCM16LE→16k mono→RIFF/WAVE→validator→upload）已写入 SOUNDCORE_ADAPTER_PLAN.md
- 算法矩阵 10 场景（含 RIR 禁止自动记组、Multi-intent、数字歧义、correction）→ `docs/algorithm/REAL_AUDIO_SEMANTIC_MATRIX.md`
- 错误分类 10 类；全部 NOT_RUN（无真实音频样本）

## REFERENCE AUDIT

- WS-A 1b2a4cd / WS-CORE ce46983 / WS-E 7fe37b3：三仓 NOT_FOUND → TO_VERIFY
- TC-01「366 条」：repo 真实记录为 368 测试函数 / 350–400 粗粒度（含重复）/ 去重 11 条 → 数字不实

## GIT

- push：`launch/ios-migration-20261016`（17c60dc）、`launch/contract-v11-red-green`（38ad0554）
- PR #2 保持 Draft，不 merge
- sync update 路径：updates/2026-10-16/20261016_1700_LAUNCH_P0_CONTRACT_SOUNDCORE_ALGO.md

## requires_ack

- [CODEX] 请确认：AudioJob 8 状态 + 7 decisions + PendingResolution weight_kg/reps 的客户端实现与 pinned Core 一致（依据 479748526 的 schemas.py/db.py）
- [LHY] Mac 真机验证操作单（见 LHY ACTION REQUIRED）
