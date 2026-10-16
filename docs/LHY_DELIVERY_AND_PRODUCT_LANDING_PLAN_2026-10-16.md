# FITCORE｜LHY交付与完整产品落地计划

**日期：** 2026-10-16  
**接收人：** LHY  
**项目：** Fitcore 智能健身语音记录系统  
**目标：** 完成真实录音豆、iPhone、AI理解、训练记录和复盘的产品闭环，并明确后续商业化试点条件。

> 协调说明：本文件按LHY提供的交付计划登记。计划中 `MASTER / WS-A / WS-CORE / WS-C / WS-E` 作为当前交付计划中的工作流/模块标识保留；除非有独立GitHub身份与ACK证据，不将其自动提升为新的开发Agent。当前跨Agent控制面仍以 **Launch** 与 **Codex** 为两个开发Agent，LHY为真人Project Owner / Developer / AWS与真机操作人。
>
> 计划内若出现具体SHA、测试结果或“已验收”表述，只有在本仓库或目标代码仓库存在可核验远端证据时才算 `VERIFIED`；否则保持 `USER_REPORTED / TO_VERIFY`，不得据此直接merge/deploy。

## 一、核心结论

目前Fitcore已经有软件端到端闭环成果，但还不能认定为完整真实硬件产品。

最终目标：

**soundcore录音豆 → iPhone接收录音 → WAV上传 → 真实ASR转写 → AI语义解析 → 训练事实验证 → SQLite持久化 → 手机训练记录与复盘。**

LHY负责真实设备、真机环境、SDK与授权、人工裁定和最终验收；其余研发工作由Launch及计划中MASTER / WS-A / WS-CORE / WS-C工作流与Codex承担。

LHY完成所有任务后，仍需完成跨模块集成、安全入账、真实模型验证、可靠部署和联合验收，才能宣称完整落地。

## 二、目前已有的基础成果

| 模块 | 当前进展 | 是否完成真实闭环 | 核验口径 |
|---|---|---|---|
| FastAPI / Fact Engine / SQLite | Core基线 `479748526bf731b2dfe9292201394019ed7e088c`，软件闭环基线存在 | 仅软件范围 | `VERIFIED`：Core commit远端存在；当前AWS部署未完成 |
| 浏览器Web Demo | 报告称Chrome 9/9通过，`WS-E/ws-demo@7fe37b3` | Fake-ASR测试条件 | `USER_REPORTED / TO_VERIFY` |
| iOS应用 | 已迁移Swift源码至GitHub PR #2；Linux 74/74通过 | 真机E2E未验收 | `VERIFIED`：`launch/ios-migration-20261016@17c60dc...`与Draft PR #2存在；Xcode/真机未验收 |
| 算法修复 | 报告称WS-A `1b2a4cd`通过独立回归 | 尚待M2联合集成 | `USER_REPORTED / TO_VERIFY` |
| 后端候选 | 报告称WS-CORE `ce46983`通过独立回归 | 尚待M2联合集成 | `USER_REPORTED / TO_VERIFY` |
| 真实ASR / Nova | 有历史独立验证 | 当前完整真机集成未验收 | 不得等同当前E2E |
| soundcore SDK | 待实际SDK和录音豆验证 | 未完成 | `BLOCKED` |
| AWS IB-03 | 源码传输及隔离部署仍有阻塞 | 未完成 | `GATE1_BLOCKED` |
| TC-01 | 报告称366条唯一候选账面版本，人工Gold为0 | 独立模型评测未完成 | `USER_REPORTED / TO_VERIFY` |

**原则：单模块测试结果不能累加成产品验收结果。**

## 三、LHY需要负责的工作

### LHY-P0-01：真实soundcore SDK与设备

任务：确认录音豆设备可用；提供官方SDK准确版本和技术文档；确认设备发现、连接、录音、音频传输、异常恢复能力；确认设备→iPhone真实音频传输方式；配合Launch真机调试；确认SDK使用许可与分发限制。

完成标准：Fitcore客户端通过真实SDK取得录音豆实际产生的音频，不用Mock音频或官方soundcore App结果替代。

交付：`SOUNDCORE_SDK_HANDOFF.md`

### LHY-P0-02：Mac和iPhone真机构建

任务：在Mac取得指定Git版本；Xcode构建；安装到真实iPhone；验证主要页面；检查录音、网络、设备连接权限；记录错误。

完成标准：同一指定Git提交可真实构建、安装、启动、运行；Linux Swift单测不能代替Xcode验收。

交付：`IOS_REAL_DEVICE_BUILD_REPORT.md`

### LHY-P0-03：真实录音与音频传输

任务：使用soundcore录音豆录制真实训练语音；测试开始/结束、断开/重连；取得实际音频；确认采样率、声道、编码、大小；配合Launch适配后端WAV输入。

完成标准：证明真实音频确实来自录音豆且可被Fitcore后端读取。

交付：`SOUNDCORE_REAL_AUDIO_REPORT.md`

### LHY-P0-04：真实AI调用及部署授权

任务：批准真实ASR/Nova测试；确认预算上限；提供合规凭据配置方式；配合Codex完成必须由人操作的AWS SSM步骤；决定隔离部署、模型实测和公开切流审批。

完成标准：模型服务合法授权、费用边界明确，密钥不进入GitHub、聊天记录或普通报告。

特别规则：`STAGED_TRANSFER_PASS` 仅表示源码传输通过，不等于Gate 1或隔离服务验收通过；公开切流必须单独审批。

交付：`MODEL_AND_DEPLOY_APPROVAL.md`

### LHY-P0-05：关键语义数据人工裁定

任务：裁定4项DEFER→COMMIT高风险变化；裁定既有6项reason漂移争议；配合WS-C处理需要人工判断的语义歧义；监督真人音频授权/标注/隐私；批准独立评测集采样和冻结规则。

完成标准：每项裁定包含样本、上下文、决定、理由、审核记录。人工审核的合成数据仍保留合成来源；不符合独立评测标准的样本不得计为独立真人Gold。

交付：`HUMAN_SEMANTIC_ADJUDICATION.md`

### LHY-P0-06：参加完整真机联调

真实操作链：

1. 佩戴录音豆。
2. 录制训练语音。
3. iPhone接收到音频。
4. 音频上传后端。
5. 确认真实ASR和语义解析执行。
6. 确认合法事实写入数据库。
7. iPhone查看训练记录与Review。

同时至少检查：重复上传不产生第二条训练事实；不可信来源不自动入账。

交付：`REAL_HARDWARE_E2E_ACCEPTANCE.md`

### LHY-P0-07：最终Demo及故障备用

任务：真实设备完整演示视频；现场连接与网络验证；SDK失败/断网/模型超时备用方案；明确真实集成与Mock边界；确认最终演示版本。

交付：`FINAL_DEMO_SIGNOFF.md`

## 四、LHY完成后，研发侧仍需完成

| 任务 | 负责人/工作流 | 工作 |
|---|---|---|
| R01 | Launch iOS | soundcore SDK接入正式iOS，保留iPhone麦克风备用路径 |
| R02 | Launch iOS + WS-CORE | iOS HTTP合同适配、上传、Job、Facts、Finish、Review |
| R03 | WS-CORE | 接入真实ASR/Nova，确保真实模型结果进入Fact Engine |
| R04 | WS-CORE + Codex | 正式身份、会话及音频来源安全机制 |
| R05 | MASTER | 集成WS-A、WS-CORE、iOS和Web Demo正式版本 |
| R06 | WS-C | 真实音频E2E、边界、幂等、纠错、错误恢复测试 |
| R07 | Codex | 隔离服务部署、HTTPS、安全配置、恢复方案 |
| R08 | MASTER + WS-E | 固定演示版本、路演脚本、日志证据、备用视频 |

### R01：正式iOS应用

仍需：真实soundcore音频输入、WAV验证、正式后端、8种AudioJob状态、7种语义结果、Pending/纠错、Finish/Review、刷新恢复、网络失败与重复上传、Xcode真机构建。

完成标准：iPhone端从录音到Review可完整操作。

### R02：真实AI后端

必须确认：真实音频转写；动作/重量/次数/意图识别；不确定数值不凭空猜测；纠错目标正确；Fact Engine合法COMMIT；SQLite保存正式事实而非仅模型文本。

**Job `done` 不等于正式事实入账，必须以权威 `/facts` 核验。**

### R03：安全来源和身份处理

分别处理：用户登录、Session授权、客户端来源、目标说话人确认、不确定音频人工确认。

不得将 `AUDIO_TEST_SOURCE_TRUSTED=true` 当成正式产品信任依据；Bearer Token也不等于声纹验证。

### R04：跨模块正式集成

计划要求MASTER：集成WS-A `1b2a4cd`、WS-CORE `ce46983`；由CORE签收共享文件；与iOS核对固定HTTP合同；统一Browser Demo/iOS版本映射；WS-C联合回归；生成可恢复M2候选。

约束：不得直接混入未验收AUTH-01修改，不得覆盖M1冻结版本。

### R05：真实系统测试

WS-C至少验证：正常记录、多轮、数字歧义、漏记、修改上一组、跨动作纠错、其他人说话、不可信来源、网络中断/超时、重复上传/重试。

每项含输入、预期、实际、DB断言。**错误自动入账属于P0阻断。**

### R06：后端部署与外部访问

Codex与WS-CORE至少完成：隔离可复现环境、HTTPS与认证、模型凭据安全、Session隔离、DB持久化与备份、日志/追踪、恢复/回滚。

AWS IB-03可作为部署路径，但必须按独立Gate验收，不能因本地Demo通过直接切原服务。

### R07：产品体验和最终展示

用户链：训练前进入Session；训练中录音/上传/识别状态；事实确认；Finish；Review；设备断开/识别失败/不确定记录都有反馈。

后台curl成功不能替代真实用户在手机完成操作。

## 五、最终闭环验收流程

### 第一关：真实iPhone麦克风闭环

`iPhone录音 → WAV上传 → 真实ASR → 真实Nova → 来源安全判断 → SQLite Fact → iPhone Review`

先不依赖soundcore SDK，证明移动端和AI后端真实连通。

### 第二关：真实soundcore闭环

`soundcore录音豆 → SDK音频传输 → iPhone → 后端 → 正式训练记录 → Review`

通过后才具备完整硬件闭环验收条件。

### 第三关：固定真实训练场景

标准测试语音：

> 卧推60公斤做了8个，这组有点重。

预期：动作=杠铃卧推；重量=60kg；次数=8；已完成组数=1；训练容量=480kg·次；备注“有点重”仅在备注功能正式接入后显示。

要求：真实录音来源、真实模型调用可追溯、Facts正确、手机一致、重复音频不重复入账、不可信来源不默认入账。

### 第四关：外部用户试点

在完整Demo之外还需：受控分发（如TestFlight）、录音告知与同意、用户隔离、数据保留/删除/撤回、脱敏与密钥安全、监控告警、备份回滚、模型成本控制、问题处理责任人。

## 六、双方最短执行顺序

| 阶段 | LHY负责 | 研发侧负责 | 阶段成果 |
|---|---|---|---|
| 第一阶段 | Mac真机、SDK与设备交接 | iOS合同修复、M2隔离集成 | 具备真机调试条件 |
| 第二阶段 | ASR/Nova授权、合法测试音频 | 真实AI、安全来源、HTTP联调 | iPhone真实语音闭环 |
| 第三阶段 | 实际录音豆音频测试 | SDK适配器、音频格式与上传 | soundcore完整闭环 |
| 第四阶段 | 真机验收、关键语义裁定 | 全量回归、错误恢复、隔离部署 | 可稳定演示版本 |
| 第五阶段 | 最终试点与上线审批 | 隐私、安全、运维、监控、分发 | 可交给用户试用 |

建议并行执行，不必等LHY全部完成才启动真实AI与iOS合同适配。

## 七、LHY需回传材料

- `SOUNDCORE_SDK_HANDOFF.md`
- `IOS_REAL_DEVICE_BUILD_REPORT.md`
- `SOUNDCORE_REAL_AUDIO_REPORT.md`
- `MODEL_AND_DEPLOY_APPROVAL.md`
- `HUMAN_SEMANTIC_ADJUDICATION.md`
- `REAL_HARDWARE_E2E_ACCEPTANCE.md`
- `FINAL_DEMO_SIGNOFF.md`

真实音频、授权文件、SDK私有文件和密钥只走合规私有渠道；普通GitHub仅保存脱敏报告、版本与证据索引。

## 八、最终完成定义

### 真实硬件闭环完成

必须同时满足：

1. 真实soundcore录音豆产生音频。
2. 实际SDK传给iPhone。
3. iPhone成功上传真实WAV。
4. 真实ASR/Nova完成解析。
5. 来源与会话安全规则执行。
6. 正式训练事实正确入账。
7. iPhone显示正确训练记录和Review。
8. 重试/异常不产生虚假事实。
9. 所有证据来自同一明确版本。
10. MASTER与LHY共同签收（MASTER身份/工作流需在正式签收前进一步核验）。

### 可落地给真实用户

在完整硬件闭环基础上，还需达到可试点的安全、隐私、部署、可靠性、运维标准。

## 九、最终分工

**LHY：** 真实soundcore设备、Mac/iPhone真机、SDK资料与授权、真实设备验证、人工裁定、关键审批、最终验收。

**研发侧：** iOS客户端开发、SDK适配、真实AI后端、安全来源与事实入账、跨模块集成、自动化测试、隔离部署、产品体验、试点运维。

**最终目标：** 不是让每个模块分别报告“完成”，而是让一位真实用户戴上录音豆完成训练，并在手机看到正确AI训练记录与复盘，且整个过程可重复、可验证、可恢复。