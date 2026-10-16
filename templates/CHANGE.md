# [TASK-ID] 简短标题

> **复制本模板生成一份新文件，不要直接覆盖模板或他人的更新。**

```yaml
update_id: "YYYYMMDD-HHMMSSZ_AGENT_TASK_SHA7"
date_utc: "YYYY-MM-DDTHH:MM:SSZ"
agent: "LAUNCH | CODEX | LHY | CHATGPT"
task_id: "TC-01 | C03 | iOS-SDK-01 | ..."
type: "CODE | API | SDK | DATA | TEST | BUGFIX | DOC | CORRECTION"
status: "PUSHED_UNREVIEWED | REVIEWED | MERGED_NOT_DEPLOYED | DEPLOYED_VERIFIED | LOCAL_ONLY | BLOCKED | DOC_ONLY"
code_repo: "Dnlock/fitcore-core | Dnlock/fitcore-ios | ... | N/A"
code_branch: "分支名 | N/A"
code_commit: "40位实际SHA | N/A"
code_commit_url: "实际可打开的URL | N/A"
code_push_verified: false
merge_status: "NOT_MERGED | MERGED | N/A"
deploy_status: "NOT_DEPLOYED | DEPLOYED_VERIFIED | UNKNOWN | N/A"
affected_agents: ["CODEX", "LHY"]
requires_ack: ["CODEX"]
supersedes: null
```

## 1. 本次目的和结果

- **解决的问题：**
- **是否已完成：**
- **本次不包含：**

## 2. 具体变更（可审计粒度）

| 修改路径 | 之前行为 | 现在行为 | 为何修改 |
|---|---|---|---|
| `path/to/file` | ... | ... | ... |

## 3. 接口、数据与上下游影响

- 输入/输出字段、HTTP状态/音频格式/Schema发生变化？若无，明确写 `NONE`：
- 是否破坏兼容？最低兼容版本：
- 用户/Session/Fact/Pending/纠错/鉴权边界：
- Codex要做：
- LHY要做：
- Launch要做：

## 4. 测试和证据

| 命令/场景 | 环境与测试数据来源 | 结果 | 证据链接/脱敏路径 |
|---|---|---|---|
| ... | 合成 / 授权真实 / Mock | passed/failed/skipped | ... |

- **未覆盖风险/已知失败：**
- **不得将独立测试视为跨模块集成或真实准确率。**

## 5. 合并/部署状态

- 实际目标代码仓库Commit SHA及URL：
- 是否merge：
- 是否deploy：
- 如何验证版本一致性：

## 6. 下一步 / ACK

- 接收Agent：
- 必须确认的具体字段/行为：
- 阻塞条件和解法：
- 预计下一步：

## 7. 安全与数据来源

- 是否包含真实录音/隐藏人工GT/密钥/用户个人数据：**NO**（否则不要提交）
- 数据集版本与标签：
- 备注：