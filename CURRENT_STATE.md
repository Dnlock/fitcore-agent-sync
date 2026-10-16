# Fitcore 当前项目状态（人工核验快照）

**as_of：2026-10-16**。本快照是首次初始化摘要，并非所有分支最新结果的完整审计。以实际代码与新日志为准。

| 方向 | 状态 | 证据/限制 | Owner |
|---|---|---|---|
| 产品目标 | 已明确 | 声音采集→ASR→语义理解→可信Facts→Review/建议 | 用户/全员 |
| 录音硬件 | 已到手 | soundcore设备已收到；配对/赛事SDK真机链路尚未提供验收报告 | LHY |
| iOS/SDK | 待核验 | `Dnlock/fitcore-ios` 在2026-10-16查询时为空仓库；本地代码存在与否未知 | LHY |
| AWS核心 | 既有单条真实WAV闭环报告 | HTTP→ASR→Nova→Fact→Finish/Review此前已验收；当前在线版本与状态待实测 | Codex |
| Track A | 六项独立交付 | `trae/core-after-r7-20261015` 历史参考HEAD `eb9ec8b`；非生产集成声明 | Launch |
| Track B | 八项第二阶段独立交付 | F01–F08各分支/提交已有交接记录，当前NOT_INTEGRATED | Launch/Codex |
| Track C | TC-01有远端实现 | `trae/tc01-semantic-matrix-20261016`含 `tc01/`数据、Schema、validate与回归代码；TC-02..06证据待核验 | Launch |
| 独立人工评测 | 缺口仍在 | TC-01 `human_independent/README.md` 标记 `PENDING_EXTERNAL_DATA`；不应宣称独立人工准确率 | LHY |
| 数据安全 | 需持续核查 | 原始WAV、GT、密钥不进入GitHub同步仓库 | 全员 |
| GitHub | 核心/协调仓库可读取 | `fitcore-core`与`fitcore-coordination`私有可访问；不代表所有本地资产全已推送 | Codex |

## 当前最高优先级

1. **P0（赛事/演示）：** soundcore真机→iOS→WAV→AWS→Fact→Review真实闭环，保存脱敏验收证据及备用Demo。
2. **P0（事实安全）：** 数字、计划/完成、Pending、纠错、幂等、序列Finish在真实音频链路中不出错。
3. **P1（研发连续性）：** Launch继续Track C高风险数据与回放，Codex受控集成、LHY负责真机音频与独立人工评测。
4. **P1（跨Agent同步）：** 每个实际变更都出GitHub细粒度Markdown并要求下游ACK。

## 版本引用（需按后续更新核实）

- AWS此前稳定业务参考：`3a5cbbe`，不保证为当前线上版本。
- Track A历史提交：`eb9ec8b`。
- Track B：F01 `86a73fa`、F02 `e49c45e`、F03 `8db972d`、F04 `dab8800`、F05 `ff4199b`、F06 `a4a5791`、F07 `8d11800`、F08 `14d2c11`。
- `Dnlock/fitcore-core`主分支README仍包含早期历史语句，不作为所有后续分支集成状态的最终依据。

## 责任和编号

- Launch代替TRAE，保留原A/F/TC系列分支和任务号，禁止重命名抹去历史。
- Codex继续`C01–C07`以及HTTP/部署/接口集成。
- LHY继续iOS、硬件与人工评测。
- ChatGPT做交接校核与状态总结；未经授权不合并/部署。