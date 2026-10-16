# Launch iOS Parallel Track — Contract Integration + Device Readiness + Soundcore Preparation

```yaml
update_id: "20261016_2245_LAUNCH_IOS_PARALLEL_TRACK"
date_utc: "2026-10-16"
agent: "LAUNCH"
type: "CODE"
status: "PRODUCTION_INTEGRATION_TESTS_DONE"
code_repo: "Dnlock/fitcore-ios"
code_branch: "launch/contract-v11-production-integration"
code_commit: "bf12cbd36e32b57a1da874b8bee7e5ac53f050e5"
code_push_verified: true
merge_status: "NOT_MERGED（Draft stacked branch）"
requires_ack: ["CODEX", "LHY"]
```

## 本轮完成

1. **Lineage audit**：`docs/migration/IOS_SOURCE_LINEAGE_2026-10-16.md`（canonical = 17c60dc → 38ad0554；历史三目录 archive，不回覆盖）
2. **Production integration 四态审计**：`docs/contract/CLIENT_CONTRACT_V11_PRODUCTION_INTEGRATION.md`
   - DTO_SUPPORTED：session 120 / 8 状态 / 7 decisions / Pending 全字段 / correction 模型
   - PRODUCTION_WIRED：facts 权威快照（去重替换）、finish 202/409 identity 保持、review 409、done≠COMMIT
   - NOT_IMPLEMENTED（诚实）：Pending resolve 生产 UI/工作流、correction 生产调用、7 decisions 与 job 状态的差异化 UI
3. **Tests**：RED-06 独立 test + 9 生产集成 tests + 7 音频规范化 tests
   - Linux 全绿：**185 tests / 0 failures / exit 0**
   - 分支：`launch/contract-v11-production-integration` @ `bf12cbd`
4. **音频**：`AudioCaptureBackend` 抽象（IPhoneMic fallback + 未来 SoundcoreAudioBackend）；`WAVNormalizer`（48k stereo→16k mono、PCM16LE、RIFF/WAVE、≤7MB、确定性输出）；源文件保留 + SourceAudioMetadata 脱敏模型
5. **Soundcore**：SDK_PACKAGE_REQUIRED（未猜 API）；适配层协议与文档字段对齐已确认
6. **Mac 操作单**：更新绑定最新 SHA `bf12cbd`

## Codex ACK（informational）

- 已知 SOURCE_TRANSFER = STAGED_TRANSFER_PASS（runtime_files=30）
- 已知 Gate1 deployment = BLOCKED / NOT_RUN（BROWSER_CONTROL_INTERRUPTED）
- Real HTTP 继续 gated；不请求 secret；不进行真实 AWS 调用
- 待 Codex 返回 GATE1_PASS / NEXT_STATE=GATE2_APPROVAL_REQUIRED 后才申请 Real HTTP 联调

## LHY ACTION REQUIRED

1. Mac 真机验证（bf12cbd）：verify.py → xcodebuild → simulator smoke → physical iPhone
2. Soundcore iOS 包：iosFrameworkExport.zip v1.3.01（SDK_PACKAGE_REQUIRED）
3. Gate1：由 Codex 手动部署（Launch 不触碰 AWS）
