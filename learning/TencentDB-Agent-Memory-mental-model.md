# TencentDB-Agent-Memory 心智模型

> 用途：累积记录七阶段学习中已经由学习者复述并确认的结论。未经确认的内容只进入“待确认问题”，不作为定论。

## 证据基线

- 学习阶段：第 1 阶段——系统全景、问题空间和竞品比较基线。
- 预检日期：2026-08-18。
- 本地分支：`feat/server_team`。
- 本地基线 commit：`97f94654280b2932c35ba4806a491999ed244cc9`，提交时间 2026-08-15。
- Remote：`https://github.com/TencentCloud/TencentDB-Agent-Memory.git`。
- 本阶段阅读的是该 commit 下的根 `README_CN.md`、`MemoryCore/README_CN.md`、`MemoryKnowledge/README.md`、`MemoryProxy/README_CN.md`、`MemoryPanel/README.md` 与 `CHANGELOG.md`。
- 最终写入前，工作区出现了预检后产生的大量外部修改与删除；本阶段结论仍绑定上述 commit，不把后续脏工作区当成证据。
- 本阶段未读源码、未运行测试、未安装或部署。因此没有 `[源码确认]` 和 `[测试确认]` 结论。

## 学习进度

- 阶段 1：已通过（2026-08-18）
- 阶段 2～7：未开始

## 已确认结论

### 1. 问题空间

- `[项目文档]` TencentDB Agent Memory 的出发点是减少 Agent 在新 Session、新 Agent和团队协作中的重复学习，把对话、任务、文档和代码加工成可复用资产。
- `[推断]` 独立长期记忆不是更长的聊天记录，也不只是另一个向量库，而是位于模型上下文之外，负责捕获、提炼、分类、治理、召回、注入、更新和撤销经验的生命周期系统。
- `[推断]` 长期记忆不取代聊天历史、摘要或 RAG：
  - 最近聊天历史负责当前会话连续性；
  - 滚动摘要负责低成本恢复大意；
  - 普通 RAG负责从外部语料检索相关片段；
  - 长期记忆进一步处理跨 Session经验、类型化资产、权限、版本、装配和失效。
- `[推断]` 单人、短周期、静态文档问答通常可优先使用聊天摘要加普通 RAG；独立记忆系统增加了抽取、存储、同步、权限、延迟和错误记忆成本。

### 2. 四类经验资产

统一案例：Builder与 Reviewer跨多个 Session迁移旧鉴权模块；移动端 7.2前仍依赖 `/v1/login`，团队采用分阶段切流，曾错误怀疑 Redis，最终确认故障来自 Token时钟偏差，发布与回滚流程已经实际跑通。

| 资产 | 回答的问题 | 鉴权案例 | 不应承担 |
| --- | --- | --- | --- |
| Chat Memory | 过去发生了什么，现在必须记住什么？ | 旧移动端约束、用户要求变更可回滚、历史事故与决策 | 完整操作手册、代码调用图 |
| Skill | 这类任务怎样可靠完成并验证？ | 带触发、步骤、资源、检查、失败处理和回滚的发布流程 | 只有背景介绍而无执行协议的普通文档 |
| Wiki | 系统或领域正式是什么、为什么这样设计？ | 鉴权 ADR、兼容矩阵、运维说明 | 个体偏好、实时调用关系 |
| CodeGraph | 代码在哪里、怎样连接、修改影响哪里？ | `AuthController`、`TokenVerifier`及 callers/callees | 产品决策、用户偏好、执行 SOP |

- `[项目文档]` 四类资产被统一登记和治理，但保留不同内容结构与使用方式。
- `[推断]` 四类资产没有固定调用顺序。一次任务中可能先恢复 Chat Memory约束和匹配 Skill，再按需查询 Wiki、CodeGraph，最后回到 Skill执行验证与回滚。
- `[推断]` Chat Memory与 Wiki的边界是“交互中记住的上下文”与“团队确认的正式知识”；Skill与 Wiki的边界是“解释”与“可执行、可验收”；Wiki与 CodeGraph的边界是“设计语义”与“当前代码结构”。
- `[推断]` Secret不应进入 Prompt或长期记忆；未验证或已证伪假设可保留在 L0作为过程证据，但不能晋升为稳定事实。大量日志通常只保存关键摘要、证据定位或外部链接。

### 3. 六个责任单元

| 单元 | 主要职责 | 所属层面 | 明确不负责或需注意 |
| --- | --- | --- | --- |
| MemoryCore | L0～L3 Chat Memory、Skill、User/Team/Agent/Task/Asset/ACL及 Knowledge元信息 | 记忆数据面＋管理面 | 不运行 Agent；不保存或处理 Wiki、CodeGraph实际内容 |
| MemoryKnowledge | 文档到 Wiki、仓库到 CodeGraph的构建、索引、同步和工具查询 | 知识数据面＋后台构建 | 不承担完整团队身份、ACL和 Agent装配治理 |
| MemoryProxy | 模型协议转发、鉴权、Session初始化、召回编排、注入、响应返回和对话回流 | 接入层＋在线数据面编排 | 不保存正式 Memory Asset；其数据库主要存运行状态和缓存 |
| SDK / Adapter | 显式 capture、recall和有边界的 inject | 接入层 | 通常是 Proxy自动接入的替代方案，但可与 Proxy组合使用 |
| MemoryPanel | Web展示、团队和资产操作、绑定、权限操作、Knowledge构建入口 | 管理面＋展示层 | 文档定义为无状态控制台，业务数据由外部服务持久化 |
| 存储后端 | 分别持久化 Core业务数据、Knowledge内容索引、Proxy运行状态 | 基础设施 | 不是一个统一业务模块；Panel不是第三类业务存储 |

存储语义：

- Core Storage：Chat Memory、Skill、User/Team/Agent及资产元数据；本地文档描述默认 SQLite和本地文件。
- Knowledge Storage：Wiki页面、FTS5、链接图和代码索引。
- Proxy Runtime State：Session、注入缓存、Skill状态或版本 Pin；可使用 Redis、COS、SQLite、FS或 Memory。

### 4. 关键术语

- `capture`：捕获并保存新发生的原始交互，例如将一轮对话写为 L0。
- `recall`：根据身份、权限、当前问题和预算，从已有记忆中选择相关内容。
- `inject`：把召回结果放入 Prompt，或把查询能力作为工具提供给模型。
- `distill`：将原始或低层记忆提炼成更稳定的 L1/L2/L3或 Skill候选。
- 关键区别：`recall`决定“取什么”，`inject`决定“怎样让模型使用”。

## 已确认流程

### 1. 管理面准备

```text
管理员
  → MemoryPanel
    → MemoryCore Meta：创建 Team / User / Agent / Task，设置 Owner、ACL和资产绑定
    → MemoryKnowledge：发起 Wiki ingest或 CodeGraph build / sync
```

### 2. Proxy在线请求与工具循环

```text
用户
  → Agent Runtime
    → MemoryProxy
      → MemoryCore：验证身份，解析 Team / Agent / Task和资产范围
      → MemoryCore：召回相关 Chat Memory与 Skill
      → Proxy：按预算注入必要记忆、Session上下文和工具说明
      → 上游 LLM
        → 若需要 Wiki / CodeGraph：返回工具调用意图
          → Agent Runtime调用 MemoryKnowledge /v3/tools/list 或 /v3/tools/call
          → Agent将工具结果带入下一次模型请求
        → 生成最终响应
      → Agent Runtime
  → 用户
```

- `[项目文档]` 权限/绑定范围与相关性检索是两个不同问题：先确定有资格访问什么，再从允许范围中选择当前问题相关内容。
- `[项目文档]` L2/L3等高层背景与匹配 Skill可直接注入；L0/L1更多通过工具按需查询；Wiki/CodeGraph通常只注入工具说明，不整库进入 Prompt。
- `[推断]` Knowledge工具通常由 Agent Runtime执行，工具结果需要进入后续模型请求；不能把它简化为 Proxy内部一次同步转发。

### 3. 对话回流与后台沉淀

```text
一轮结束
  → Proxy向 Core提交对话切片、L0写入和 Skill对话归档
    → Core保存 L0原始证据
      → 后台管线读取待处理 L0
        → L1：事实、偏好、约束、事件
        → L2：项目或场景级组织
        → L3：长期画像、稳定模式和高层认知
        → 满足条件时生成或更新 Skill候选
```

- `[项目文档]` 响应返回不等于 L1/L2/L3已经更新；后台提炼存在一致性窗口。
- `[项目文档]` Wiki由文档上传、拉取或重新 ingest更新；CodeGraph由仓库导入、手动 sync或可选 Auto-Sync更新。普通对话回流不会自动把讨论内容变成最新 Wiki或代码结构。
- `[推断]` L0应保留为派生记忆的原始证据，便于核对、重新生成或撤销错误结论。

### 4. SDK / Adapter路径

移除 Proxy后，Agent Runtime或 Adapter需要接管：

1. 构造 Prompt前 recall；
2. 将召回结果有边界地 inject；
3. 一轮完成后 capture L0。

Core、Knowledge和后台提炼仍可保留；变化的是在线编排主体。

## 对比坐标

### 1. 聊天历史、摘要、普通 RAG和独立记忆

| 基线 | 主要能力 | 主要缺口或代价 |
| --- | --- | --- |
| B0 最近聊天历史 | 当前会话连续性 | 窗口有限、跨 Session弱、按时间而非长期价值组织 |
| B1 历史＋滚动摘要 | 低成本恢复大意 | 有损压缩；事实、假设、流程和关系混为同一文本 |
| B2 普通向量 RAG | 从外部语料检索相关片段 | 默认不包含经验提炼、版本化 Skill、代码图和完整治理 |
| M 类型化 Agent Memory | 跨 Session、类型化资产、治理和选择性上下文 | 增加错误提炼、过期、同步、ACL、延迟和运维成本 |

- `[推断]` 如果 RAG已经增加事实抽取、ACL、Agent Binding、版本化 Skill、Wiki链接图、代码调用图和审核生命周期，它已超出这里定义的“普通向量库 RAG”，逐步形成记忆平台。

### 2. 待验证假设

| 假设 | 对照和指标 | 否证条件 |
| --- | --- | --- |
| H1 分层 Chat Memory提高跨 Session约束遵守率 | 对比滚动摘要；测约束违反、事实召回、错误召回、Token | 摘要相同或更好且更低成本 |
| H2 Skill降低重复任务探索成本 | 对比只检索历史/Runbook；测成功率、Turns、工具数、遗漏、延迟 | Skill无改善或过期 Skill降低成功率 |
| H3 Wiki结构改善跨文档问题 | 对比相同语料的平面 RAG；测正确率、引用覆盖、噪声、延迟 | 普通 RAG相同或更好且更低成本 |
| H4 CodeGraph改善代码影响分析 | 对比代码文本检索；测 caller/callee召回、漏项、错误修改 | 原生搜索或代码 RAG同等效果且更便宜 |
| H5 ACL＋Binding降低上下文污染 | 对比 namespace/metadata filter；测越权、错项目召回、Token和漏召回 | 普通过滤已达到同等隔离与成本 |
| H6 后台提炼降低在线延迟但引入一致性窗口 | 测首 Token、总延迟、提炼延迟和下一轮缺失率 | 新信息长期不可见且最近历史无法弥补 |

所有假设实验都应固定模型、任务、Prompt、语料/代码版本、工具、上下文预算和运行环境，只改变主要记忆机制。

### 3. Mem0、Zep、Letta高层坐标

外部资料为 2026-08-18 官方在线文档快照；Zep文档明确为 v2，Mem0与 Letta页面未给固定页面版本号。

| 坐标 | TencentDB Agent Memory | Mem0 | Zep | Letta |
| --- | --- | --- | --- | --- |
| 设计中心 | 外部 Agent的团队经验资产与治理 Hub | 可嵌入应用的记忆层/API | 时间感知的上下文图和上下文组装 | 拥有持久身份与记忆的 Agent Runtime |
| 主要单位 | Chat Memory、Skill、Wiki、CodeGraph；Chat分 L0～L3 | `add`产生或写入的记忆记录，按 User/Agent/Run分区 | Episode、Entity、带时间区间的 Fact/Edge；用户图与组图 | Stateful Agent及 Git-backed MemFS、对话和 Skill |
| 写入主体 | Proxy/Adapter捕获；Core提炼；人治理；文档/代码独立 ingest/sync | 应用调用 `add`，引擎抽取；开发者可管理记录 | 应用每轮 `memory.add`，业务数据 `graph.add`，服务构图 | Agent自行学习更新、用户 `/remember`、Dreaming后台整理 |
| 检索/上下文 | 分层召回、Skill匹配、知识工具；ACL＋Binding和预算 | `search`＋ID/metadata filter，可配置混合检索/reranker | `memory.get`生成 context string；`graph.search`查询图 | Agent读写 MemFS/项目文件；附加或分离共享仓库 |
| 共享/治理 | Owner、版本、状态、private/team/restricted/agent | User/Agent/Run作用域；自托管 Server有 API Key、Dashboard、审计 | 用户图跨 Session；Group Graph存共享或非用户数据 | Agent私有 MemFS；组织 Shared Memory Repository附加给多个 Agent |
| 部署 | 当前仓库提供 Core、Hub、Proxy等自托管组件 | Python/Node库或自托管 Server；另有托管平台 | Zep托管或客户云；Graphiti是自托管 OSS引擎 | Letta Cloud、本地 Runtime或自托管 App Server；当前共享仓库要求 Cloud Agent |

- `[竞品官方文档]` Mem0 OSS概览：<https://docs.mem0.ai/open-source/overview>
- `[竞品官方文档]` Zep v2 Key Concepts：<https://help.getzep.com/v2/concepts>
- `[竞品官方文档]` Graphiti官方仓库：<https://github.com/getzep/graphiti>
- `[竞品官方文档]` Letta Stateful Agents：<https://docs.letta.com/concepts/stateful-agents>
- `[竞品官方文档]` Letta Memory & Dreaming：<https://docs.letta.com/configuration/memory>
- `[竞品官方文档]` Letta Shared Memory：<https://docs.letta.com/concepts/shared-memory>
- `[竞品官方文档]` Letta Self-hosting：<https://docs.letta.com/self-hosting>

不能混淆：TencentDB CodeGraph的核心对象是代码符号和调用关系；Zep/Graphiti时间图的核心对象是实体、事实、关系及其时间有效性。Letta的 Skill与 Agent Runtime和 MemFS结合；TencentDB文档则把 Skill强调为可跨 Agent装配和治理的版本化资产。

## 迁移思路及适用前提

### 可迁移思想

`[推断]` 不要只建设一个无限增长的向量库。先按信息的使用语义区分：

- 历史约束与偏好；
- 可执行且可验证的策略；
- 正式领域知识；
- 当前系统、代码或环境关系。

然后分别设计写入主体、证据、验证、版本、权限、召回、注入、更新和失效机制。

迁移到 VLA可类比为：

| Agent Memory资产 | VLA迁移对应 |
| --- | --- |
| Chat Memory | 操作者偏好、任务约束、历史失败条件 |
| Skill | 已验证操作策略、恢复流程和验收规则 |
| Wiki | 机器人、场景、任务规范和安全说明 |
| CodeGraph思想 | 场景图、对象关系图、技能依赖图或系统调用图 |

适用前提：

1. 任务跨多个 Session或 Episode持续；
2. 确实存在可复用经验；
3. 能区分原始证据、已验证事实、工作假设和模型推断；
4. 资产具备来源、版本、更新和失效机制；
5. 召回不能绕过实时观测、环境反馈和安全检查；
6. 错误记忆的代价可以通过审核、回滚或隔离控制。

## 待确认问题

1. `[项目文档]` Core README将 L2/L3描述为 Scenario与 Core/Persona；Proxy README又用 Agent Profile与 Team/Global解释 L2/L3。需要确认这是内容层级与注入作用域的不同口径，还是版本不一致。
2. `[项目文档]` Proxy README一处称对话切片“同步发到” Core，请求流程又称“异步回流”。需要源码与真实链路确认回流是否阻塞响应、怎样重试。
3. Chat Memory、Skill的自动抽取分别由什么事件触发，哪些默认启用，哪些需要显式配置？
4. Skill候选是否必须审核后才进入实际召回？版本选择与回滚规则是什么？
5. 同一事实在 L1/L2/L3、Wiki或 Skill之间冲突时怎样检测、选择、更新和撤销？
6. ACL、Fixed Binding、相关性检索和上下文预算在主路径中的精确执行顺序是什么？
7. Wiki与 CodeGraph的 ready、stale、failed状态怎样影响 Agent工具暴露和查询？
8. Proxy知识工具调用究竟由 Agent Runtime直接执行、由 bridge转发，还是因客户端而异？
9. README中的 PersonaMem结果尚未复现，不能作为当前 `[测试确认]` 证据。

## 证据索引

### 本地项目文档

- `[项目文档]` `README_CN.md`：问题定义、四类资产、L0～L3、Agent Loadout、按需知识工具和项目限制。
- `[项目文档]` `MemoryCore/README_CN.md`：Core边界、数据面/管理面 API、Adapter职责、存储与隔离。
- `[项目文档]` `MemoryKnowledge/README.md`：Wiki/CodeGraph构建、索引、工具与 Auto-Sync。
- `[项目文档]` `MemoryProxy/README_CN.md`：透明代理、身份、注入、工具说明、对话回流和 ProxyStorage。
- `[项目文档]` `MemoryPanel/README.md`：无状态 Control、展示、聚合和公开管理 API。
- `[项目文档]` `CHANGELOG.md`：本地顶部为 `2.0.1-beta.1`，但官方分支历史已出现 `2.0.1-beta.2`提交，因此使用 commit SHA作为基线。

### 当前证据边界

- `[源码确认]`：无。本阶段按规则没有阅读源码。
- `[测试确认]`：无。没有运行单元、集成、E2E、Benchmark或部署验证。
- `[推断]`：系统平面划分、可证伪假设、迁移到 VLA的映射及部分设计原因是教学分析，不是项目已证明事实。
