# [V2-INTEGRATION] V2 算法+后端集成候选（F01-F08 Flag 关闭）

```yaml
update_id: "20261017-042500Z_LAUNCH_V2-INTEGRATION_3d0656d"
date_utc: "2026-10-17T04:25:00Z"
agent: "LAUNCH"
task_id: "V2-INTEGRATION"
type: "CODE"
status: "PUSHED_UNREVIEWED"
code_repo: "Dnlock/fitcore-core"
code_branch: "integration/fitcore-v2-algorithm-backend-20261017"
code_commit: "3d0656dbea0c929ff65dbead5f01023d467d23b0"
code_commit_url: "https://github.com/Dnlock/fitcore-core/pull/3"
code_push_verified: true
merge_status: "NOT_MERGED"
deploy_status: "NOT_DEPLOYED"
affected_agents: ["CODEX", "LHY"]
requires_ack: ["CODEX", "LHY"]
supersedes: null
```

## 1. 目的与结果
- V2 候选：M1 4797485 + M2 29abbf9 + G-04 白名单 + F01-F08 全八模块 features 子树合入（全部 Feature Flag 默认关闭）。
- 测试：1187 passed / 3 skipped（本机 pytest，59.92s）。远端 SHA 回读一致。
- 安全：D2-01=VERIFIED_FIX；AUTH-01 legacy 绕过修复在独立分支（不在此 PR）；生产接线待批准。

## 2. 变更
- 5 新提交，59 文件 +6897/-3：app/config.py+main.py（G-04）、app/features/{training_relevance,subjective_state,training_memory,next_set_recommendation,training_qa,acoustic_events,speaker_diarization,speaker_verification}/、各模块测试。

## 3. 接口影响
- wire 协议无变化（Flag 关闭）；iOS 12 项矩阵 p00-p10 补丁独立于本 PR。
- Codex 要做：后端合同/安全门禁审查。
- LHY 要做：iOS 兼容确认（联调前置=Swift 修复+Mac 构建）。

## 4. 测试证据
- 命令：PYTHONPATH=. .venv-core-real/bin/python -m pytest -q → 1187 passed, 3 skipped。
- 数据来源：合成/确定性 provider；无真实 ASR/Nova（NOT_VERIFIED）。

## 5. 合并/部署
- NOT_MERGED / NOT_DEPLOYED（Draft PR #3；不自动合并 main；AWS NOT_RUN）。

## 6. ACK
- 接收：CODEX、LHY。待其按协议回 ACK。

## 7. 安全
- 无密钥/私人音频/私有 SDK（扫描通过）；AUTH-01 敏感复现细节不在此公开仓库。
