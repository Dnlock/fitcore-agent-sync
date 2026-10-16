# [SYNC-BOOTSTRAP] 建立独立Agent同步规则

```yaml
update_id: "20261016-0000_CHATGPT_COORD_BOOTSTRAP"
date_utc: "2026-10-16"
agent: "CHATGPT"
task_id: "SYNC-BOOTSTRAP"
type: "DOC"
status: "DOC_ONLY"
code_repo: "N/A"
code_branch: "N/A"
code_commit: "N/A"
code_push_verified: false
merge_status: "N/A"
deploy_status: "N/A"
affected_agents: ["LAUNCH", "CODEX", "LHY"]
requires_ack: ["LAUNCH", "CODEX", "LHY"]
```

## 改了什么

- 制定跨Agent的统一读写顺序、细粒度Markdown日志、版本状态枚举和ACK协议。
- 增加当前项目事实快照、后端接口登记和三个Agent的可复用任务规则。
- 每次新增代码/接口/SDK/数据行为必须有可验证代码SHA和同步报告；支持无代码的BLOCKER或纯文档事件。

## 没做什么

- 没有修改 `fitcore-core` / `fitcore-ios` 任何代码。
- 没有合并、部署或执行完整真机测试。
- 不代表已经向所有Agent下发指令或它们已实际安装/启用GitHub权限。

## 下一步

- 人类已创建公开`Dnlock/fitcore-agent-sync` GitHub仓库并同意公开存放脱敏协作资料；发布本包Markdown后核验。
- 将 `docs/AGENT_PROMPTS.md` 分别发送给Launch、Codex和LHY Agent，并为相应账号/工具授予仓库访问权限。
- 三方首轮同步后各自提交一份`ACK`和真实版本状态，协调者据此更新 `CURRENT_STATE.md`。