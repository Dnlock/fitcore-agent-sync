# ACK — 对更新记录的接收确认

```yaml
ack_id: "YYYYMMDD-HHMMSSZ_AGENT_TARGET-UPDATE"
receiver: "CODEX | LAUNCH | LHY"
source_update_id: "被确认的update_id"
source_change_url: "同步仓库中的更新Markdown链接"
source_code_repo: "已实际打开的目标仓库"
source_code_sha: "已读的精确SHA"
ack: "ACCEPTED | NEEDS_CHANGE | BLOCKED | INFO_ONLY"
date_utc: "YYYY-MM-DDTHH:MM:SSZ"
```

## 我实际核对了什么

- 文件/接口/版本：
- 兼容性检查：
- 我方代码是否需要修改：
- 是否存在契约冲突或无法验证项：

## 下一步负责人和动作

- Owner：
- 动作与验收证据：
- 是否允许集成：**未经人类明确授权，ACK不构成merge/deploy许可。**