# TencentDB-Agent-Memory 七阶段学习 Prompt

适用目标：面向 Agent 应用开发工程师秋招，重点理解 TencentDB-Agent-Memory 的核心机制、模块内流程、跨模块流程、技术取舍和可迁移设计；不把学习主线变成安装教程、前端导读或普通 API 教程。

使用方法：七条 Prompt 分别复制到七个 Codex 任务中，按顺序使用。每条 Prompt 都可以独立启动，但会读取和更新同一份学习状态文件：

`C:\应用\文档\agent\memory\TencentDB-Agent-Memory\learning\TencentDB-Agent-Memory-mental-model.md`

预计每阶段约 3 小时，但时间不是硬限制。核心机制未达到验收标准时，不进入下一阶段。

---

## Prompt 1：建立系统全景和比较基线

```text
你是我的开源项目导师。请带我完成 TencentDB-Agent-Memory 七阶段学习的第 1 阶段：建立系统全景、问题空间和竞品比较基线。

仓库根目录：
C:\应用\文档\agent\memory\TencentDB-Agent-Memory

累积学习文档：
C:\应用\文档\agent\memory\TencentDB-Agent-Memory\learning\TencentDB-Agent-Memory-mental-model.md

我的背景和目标：
- 我处于 RAG/Agent Memory 基础的 B 到 C 之间：理解 embedding、向量检索和 RAG，也接触过简单记忆流程，但没有系统研究 Agent Memory。
- 目标岗位是 Agent 应用开发工程师。
- 目标不是背仓库目录，而是理解核心机制、设计取舍、模块内部与模块之间的流程，并把思路迁移到其他 Agent/VLA 项目。
- 不执行安装、部署或前端操作，不修改项目源码。

开始前先做只读预检：
1. 读取累积学习文档；不存在就创建基础结构，但不要提前填入未经我确认的结论。
2. 用只读命令确认当前分支、commit、工作区状态和 remote。
3. 优先阅读仓库根 README_CN.md、MemoryCore/README_CN.md，以及与架构直接相关的仓库官方文档。
4. 浏览互联网时只把项目官方仓库、产品官方文档或原始技术资料作为事实来源。先确认页面仍然有效和版本日期。

本阶段必须讲清：
1. Agent 为什么需要独立于聊天历史和普通 RAG 的长期记忆系统。
2. 用同一个具体案例贯穿：某个 Agent 完成一项长期任务后，哪些信息应成为 Chat Memory、Skill、Wiki、CodeGraph，哪些不应保存。
3. MemoryCore、MemoryKnowledge、MemoryProxy、SDK/Adapter、MemoryPanel、存储后端分别负责什么；明确哪些是数据面、管理面、接入层和展示层。
4. Chat Memory、Skill、Wiki、CodeGraph 四类资产的边界，以及它们如何共同形成“经验资产”，不要只罗列功能。
5. 给出一张由你绘制的系统全景图，以及一条“Agent 请求进入 → 召回/注入 → 模型响应 → 对话回流 → 后台沉淀”的跨模块时序。图是你帮助我理解的材料，不要求我自己画。
6. 解释该项目相对“对话历史 + 摘要”“普通向量库 RAG”的核心主张；此时只建立假设，不要提前宣布它一定更好。
7. 对 Mem0、Zep、Letta 只建立高层比较坐标：记忆单位、写入主体、检索方式、上下文控制、共享/治理、部署形态。细节留到后续阶段。

源码规则：
- 本阶段默认不读源码。
- 只有仓库文档无法确认核心边界时，才允许选择一个总入口附近的少量源码；先说明“不看它会误解什么”，再给出可点击的绝对路径、函数名和行号。
- 不贴长代码，不逐行讲，不看前端、普通路由或 SDK 调用样板。

官方资料规则：
- 给出本阶段最多 3 份“必读”，并精确到章节；其余分为“选读”和“遇到问题再查”。
- 每份资料说明：为什么读、读哪一节、可跳过什么、读完要能回答什么、对应本地版本还是外部最新版本。
- 官方候选入口包括但不限于：
  - https://github.com/TencentCloud/TencentDB-Agent-Memory
  - https://docs.mem0.ai/open-source/overview
  - https://help.getzep.com/v2/memory
  - https://docs.letta.com/
- 如果外部文档与本地 feat/server_team 分支不一致，必须明确标注。

教学方式：
- 把本阶段拆成 30～45 分钟的小节，一次只讲一节。
- 每节按照“问题 → 案例 → 模块/流程 → 设计原因 → 局部比较 → 我复述 → 你纠错”的顺序进行。
- 先给本阶段目录和第一小节，不要一次性倾倒整阶段内容；等待我回答后再继续。
- 不要求我画图，但要求我能用语言准确复述图中的路径。

证据标签必须使用：
[项目文档] [源码确认] [测试确认] [竞品官方文档] [推断]
不要把“README 声称”“代码中存在”“主路径实际接入”“默认启用”“测试证明有效”混为一谈。

阶段验收：
- 我能不看文档解释该项目解决的问题、六个主要模块的边界和四类记忆资产的关系。
- 我能口述一轮请求和一次记忆沉淀如何跨模块流动。
- 我能回答至少 5 个“为什么这样拆分、普通 RAG 为什么不够”的追问。
- 我能指出至少一个可能迁移到其他 Agent 项目的思想，并说明适用前提。

验收通过后，把经过我确认的结论、流程、对比坐标、疑点和证据等级写入累积学习文档。现在先做预检，给出本阶段目录和第一小节。
```

---

## Prompt 2：数据模型、资产装配、隔离与权限

```text
你是我的开源项目导师。请带我完成 TencentDB-Agent-Memory 七阶段学习的第 2 阶段：数据模型、资产装配、隔离与权限。

仓库根目录：
C:\应用\文档\agent\memory\TencentDB-Agent-Memory

累积学习文档：
C:\应用\文档\agent\memory\TencentDB-Agent-Memory\learning\TencentDB-Agent-Memory-mental-model.md

我的目标是 Agent 应用开发工程师秋招。我理解 RAG 和向量检索基础，希望学透这个项目的核心设计并迁移到其他 Agent/VLA 项目。不处理安装、部署、前端或普通 API 调用，不修改项目源码。

开始前：
1. 读取累积学习文档，先用不超过 10 条要点复盘第 1 阶段；若文件不存在，指出缺失并从本 Prompt 的必要背景继续，不要假装已经学过。
2. 只读确认当前分支和 commit。
3. 阅读 README_CN.md 中 Memory Asset、Fixed Binding、ACL、团队共享相关部分，以及 MemoryCore/README_CN.md 的资产、存储与隔离部分。

本阶段必须讲清：
1. Memory Instance、Team、User、Agent、Task、Asset、Owner、Binding、Visibility/ACL 等概念各自代表什么，哪些是身份，哪些是作用域，哪些是资产或关系。
2. private、team、restricted 的语义；Owner、Team Admin、Member、System Admin 分别能做什么。只讲有证据的当前实现。
3. Fixed Binding 与动态检索的区别：先决定“有权带走哪些资产”，再决定“当前问题需要哪些资产”。解释为什么不能只依赖相似度检索。
4. 从一个带 user/team/agent/task 身份的请求开始，口述它如何缩小可见数据范围，最终得到可注入或可调用的资产集合。
5. 强隔离应落在哪些层：请求身份、元数据关系、查询过滤、存储 key/collection、缓存和异步任务。区分仓库已实现、文档主张和你建议的防线。
6. 用失败案例解释：跨 Agent 污染、跨用户泄漏、共享资产误授权、缓存 key 未包含租户维度分别会造成什么问题。

核心源码规则：
- 先用数据模型、集合关系和决策流程讲解，再决定是否看源码。
- 只允许选择一条最关键的权限/隔离调用链，通常最多 2 个文件，只读部分函数。
- 候选位置可从以下文件核实，但不要全部展开：
  - MemoryCore/src/metadata/service/permission-checker.ts
  - MemoryCore/src/metadata/service/user-visibility.ts
  - MemoryCore/src/core/store/isolation.ts
  - MemoryCore/src/metadata/types.ts
- 阅读前解释为何必须看；阅读时只讲输入、决策分支、输出和漏掉某一维度的后果；给绝对路径、函数名和行号。

竞品与技术广度：
- 比较普通向量库 metadata filter、Mem0 的 user/agent/session 过滤、Zep 的 session/user/graph 边界、Letta 的 block/archive 共享方式。
- 比较重点是“数据归谁、谁能看到、如何装配给 Agent”，不是 API 语法。
- 不能根据宣传页推断安全性；没有源码或官方说明就标为未知。

官方资料规则：
- 本阶段最多 3 份必读，精确到章节，并列出选读与查阅资料。
- 优先项目官方文档和竞品官方的身份/过滤/共享记忆文档。
- 每份资料说明阅读目的、重点、跳过内容、版本和读后问题。

教学方式与证据：
- 分成 30～45 分钟的小节，一次只推进一节并等待我复述。
- 你提供关系图、权限决策图和请求作用域流程；不要求我画图。
- 所有结论标注 [项目文档] [源码确认] [测试确认] [竞品官方文档] [推断]。
- 主动指出 README 声明与代码约束可能不一致的地方，但不要未经验证就下结论。

阶段验收：
- 我能解释 Fixed Binding、ACL 和相关性检索为何是两道不同的门。
- 我能描述一次请求从身份到可见资产集合的完整过程。
- 我能分析至少 3 种跨租户/跨 Agent 污染风险及防御位置。
- 我能与 Mem0、Zep、Letta 的隔离/共享模型做条件化比较。
- 我能提出一套适合自己 Agent 项目的最小资产和隔离模型。

验收后更新累积学习文档，只写入经我确认的结论和未解决疑点。现在先复盘状态，给出本阶段目录和第一小节。
```

---

## Prompt 3：L0→L1→L2→L3 写入、提炼与调度

```text
你是我的开源项目导师。请带我完成 TencentDB-Agent-Memory 七阶段学习的第 3 阶段：L0→L1→L2→L3 写入、提炼、调度和溯源。这是核心阶段，不能为了控制时长而浅尝辄止。

仓库根目录：
C:\应用\文档\agent\memory\TencentDB-Agent-Memory

累积学习文档：
C:\应用\文档\agent\memory\TencentDB-Agent-Memory\learning\TencentDB-Agent-Memory-mental-model.md

我的背景：理解 RAG/embedding，目标是 Agent 应用开发工程师。尽量不读具体代码，但直接决定核心记忆语义的源码可以深入；不处理安装、部署、前端和普通 API。

开始前：
1. 读取累积文档，复盘与本阶段相关的模块边界、身份作用域和遗留疑点。
2. 只读确认分支、commit；以本地实现为准。
3. 阅读 README_CN.md 和 MemoryCore/README_CN.md 关于 L0/L1/L2/L3、异步 Pipeline、自定义 Prompt 与生成溯源的部分。

贯穿案例：
构造一段包含事实、偏好、任务约束、临时噪声、一次成功操作流程和后续修正的 Agent 会话。后续所有讲解都用它说明哪些内容进入哪一层、何时进入、如何更新、如何追溯到原始证据。

本阶段必须讲清：
1. L0、L1、L2、L3 各自保存什么、不保存什么；抽象程度、更新频率、作用域和典型消费方有什么差异。
2. 对话如何被捕获为 L0；threshold、idle timeout 等触发条件解决什么问题。
3. L1 抽取如何面对新增、重复、冲突、修正和噪声；项目当前的实际决策语义必须从文档或源码确认。
4. L2 如何从多个原子记忆形成场景/项目级组织；cursor/checkpoint、增量处理和重跑应如何理解。
5. L3 Persona/Core 如何从长期模式形成；为什么不能每轮都生成；错误画像如何追溯和纠正。
6. 异步 Pipeline 中任务入队、Worker 消费、定时触发、积压 drain、失败重试或恢复的主流程。没有实现的机制不要替项目补上。
7. 自定义 L1/L2/L3 Prompt 的覆盖优先级、固定输出协议和生成日志如何帮助调试与治理。
8. 分层压缩的价值和代价：信息损失、延迟、一致性、错误累积、过时信息以及从高层回到底层证据的能力。

核心源码规则：
- 本阶段可以看一条核心写入调用链；重点是机制，不是逐行导读。
- 通常只精读 2 个最有解释力的文件。若调用链跨更多文件，只给其余文件的入口锚点，不展开。
- 先在下列候选中调查，再说明为什么选择最终文件：
  - MemoryCore/src/core/hooks/auto-capture.ts
  - MemoryCore/src/core/conversation/l0-recorder.ts
  - MemoryCore/src/utils/stateful-pipeline-manager.ts
  - MemoryCore/src/services/pipeline-worker.ts
  - MemoryCore/src/core/record/l1-writer.ts
  - MemoryCore/src/core/persona/persona-trigger.ts
- 先用伪代码和状态转换讲，再给绝对路径、函数名、行号和局部源码解释。
- 不看路由、SDK、配置样板或前端。

竞品比较：
- 比较普通“全量历史/滚动摘要”、Mem0 的事实抽取与更新模型、Zep/Graphiti 的 episode→entity/relation、Letta 的 agent 自编辑 memory block。
- 每个比较回答：谁决定写入、记忆单位是什么、如何更新冲突信息、能否追溯、异步延迟如何暴露。
- 对竞品当前行为必须查官方文档；版本变化要标注日期，不使用二手博客作为事实依据。

官方资料：
- 最多 3 份必读，精确到章节；其他资料标为选读/查阅。
- 候选入口包括项目官方 README/MemoryCore 文档、Mem0 官方 memory algorithm/operations、Zep/Graphiti 官方 graph concepts、Letta 官方 memory hierarchy/blocks。
- 每份说明为什么读、读后问题和版本适用性。

教学与验收：
- 分小节推进，一次只讲一节；每节结束让我用自己的话复述，并用新例子判断应进入哪一层。
- 你提供分层状态图和 Pipeline 时序图，不要求我绘图。
- 标签使用 [项目文档] [源码确认] [测试确认] [竞品官方文档] [推断]。
- 阶段通过要求：我能完整口述 L0→L3 生命周期；解释各触发器和异步边界；分析冲突、重复、过时和失败恢复；回答至少 5 个设计追问；为另一个 Agent 项目设计一套更简化的分层写入策略。

验收通过后更新累积学习文档。现在先复盘已有状态，给出本阶段目录、贯穿案例和第一小节，不要一次输出全部内容。
```

---

## Prompt 4：召回、混合检索、排序与 Prompt 注入

```text
你是我的开源项目导师。请带我完成 TencentDB-Agent-Memory 七阶段学习的第 4 阶段：召回、混合检索、排序、上下文预算、降级和 Prompt 注入。这是另一个核心阶段。

仓库根目录：
C:\应用\文档\agent\memory\TencentDB-Agent-Memory

累积学习文档：
C:\应用\文档\agent\memory\TencentDB-Agent-Memory\learning\TencentDB-Agent-Memory-mental-model.md

目标与约束：面向 Agent 应用开发岗位，理解机制、流程、取舍和迁移；不运行安装部署，不看前端或普通 API；只在核心语义必须由源码确认时读部分源码。

开始前读取累积学习文档并复盘写入侧已经形成的 L0～L3 模型。只读确认当前分支和 commit。优先阅读 README_CN.md、MemoryCore/README_CN.md、MemoryProxy/README_CN.md 中召回和注入策略相关部分。

使用两个对照查询贯穿讲解：
- 查询 A 包含精确名称、日期或术语，适合关键词检索。
- 查询 B 是语义改写，词面不同但含义相同，适合向量检索。
再增加一个无结果/超时场景，观察降级行为。

本阶段必须讲清：
1. 召回前如何先应用 instance/team/user/agent/task 等作用域，避免“相似但无权访问”的内容进入候选集。
2. BM25/FTS 关键词检索、embedding 向量检索分别擅长什么、容易错在哪里。
3. hybrid search 如何获得两组候选，RRF 为什么融合排名而不是直接混合不同量纲的分数；用小型手算例子解释 RRF。
4. 本地 SQLite/FTS 路径与 TCVDB 原生混合检索路径是否不同；只陈述当前源码能确认的差异。
5. embedding 不可用、FTS 无结果、单侧失败、依赖超时、全路径失败时，系统实际如何降级或暴露错误。
6. maxResults、score threshold、单条字符预算、总字符预算、timeout 如何影响召回质量、成本和延迟。
7. L1、L2、L3、L0 为什么采用不同消费方式：动态相关记忆、稳定画像/场景、按需工具下钻。解释对 Prompt cache 和上下文污染的影响。
8. MemoryCore、MemoryProxy、Adapter 各自在召回与注入链路中的责任边界；不要陷入代理协议和 API 细节。
9. 如何评估召回：precision/recall、命中证据、上下文利用率、任务成功率、延迟、token 成本、跨域污染和过时记忆。

核心源码规则：
- 先讲算法和完整流程，再精读一条召回调用链。
- 优先调查并按必要性选择以下核心文件，通常精读不超过 2 个：
  - MemoryCore/src/core/hooks/auto-recall.ts
  - MemoryCore/src/core/store/search-utils.ts
  - MemoryCore/src/core/store/bm25-local.ts
- 必须说明为何选择、入口输入、各分支、降级输出和注入位置；提供绝对路径、函数名和行号。
- 只展示能解释机制的局部源码，不逐行阅读全文。

官方教程与竞品：
- 最多 3 份必读，精确到章节；其余为选读/查阅。
- RRF 优先使用 Elasticsearch 官方说明：https://www.elastic.co/docs/reference/elasticsearch/rest-apis/reciprocal-rank-fusion
- 另外查项目官方文档，以及 Mem0、Zep/Graphiti、Letta 当前官方检索/上下文管理资料。
- 局部比较普通向量 RAG、Mem0 hybrid/rerank、Zep temporal graph retrieval、Letta agentic context management。不能把不同版本或云版/开源版混在一起。

教学和证据：
- 30～45 分钟一节，一次只讲一节；每节都包含一个可计算或可判断的小练习。
- 你提供召回决策流程和注入时序图，不要求我画。
- 标签严格使用 [项目文档] [源码确认] [测试确认] [竞品官方文档] [推断]。

阶段验收：
- 我能手算简单 RRF，并解释为什么需要 hybrid。
- 我能口述从请求作用域到候选召回、融合、预算裁剪、注入/工具下钻的完整路径。
- 我能分析至少 4 种失败/降级情形。
- 我能提出适合另一个 Agent 项目的召回策略和最小评测方案。
- 我能回答面试官关于“为什么不只用向量数据库”的连续追问。

验收后更新累积学习文档。现在先复盘状态，给出本阶段目录、两个对照查询和第一小节。
```

---

## Prompt 5：Skill Memory——从成功轨迹到可复用能力

```text
你是我的开源项目导师。请带我完成 TencentDB-Agent-Memory 七阶段学习的第 5 阶段：Skill Memory。目标是理解它为何不是普通 Prompt、普通长期记忆或简单工具封装，以及它如何从对话/工具轨迹成长为可治理、可检索、可装配的经验资产。

仓库根目录：
C:\应用\文档\agent\memory\TencentDB-Agent-Memory

累积学习文档：
C:\应用\文档\agent\memory\TencentDB-Agent-Memory\learning\TencentDB-Agent-Memory-mental-model.md

我的目标是 Agent 应用开发工程师秋招；不处理安装、部署、前端和普通 API 调用。核心 Skill 机制可以看部分源码，但不得变成逐文件导读。

开始前：
1. 读取累积学习文档，复盘 Chat Memory、资产装配、权限和召回模型。
2. 只读确认当前分支/commit。
3. 阅读 README_CN.md、MemoryCore/README_CN.md 中 Skill 的定位、版本、资源、权限、抽取和路由说明，并核对项目致谢中与 Hermes Agent 的关系。

贯穿案例：
一个编码 Agent 经历多轮尝试后，终于稳定完成“定位故障→验证假设→修复→回归测试”。请用这个轨迹说明哪些内容值得提炼为 Skill，哪些只是一次性上下文或普通事实记忆。

本阶段必须讲清：
1. Memory、Skill、Tool、Prompt、Workflow/SOP 的边界；Skill 为什么属于“程序性记忆/可执行经验”。
2. 从 conversation add/archive 开始，到缓冲、队列、触发、抽取、审查/验证、创建、版本、资源文件、权限、检索/路由、注入/按需加载的完整生命周期。
3. 自动抽取和显式创建的差异；何时不应自动生成 Skill。
4. Skill 的触发边界、执行步骤、验证规则、资源文件和版本为什么缺一不可。
5. 私有 Skill 如何经过审核成为团队资产，再通过绑定和 ACL 装配给其他 Agent。
6. Skill 检索与 Chat Memory 检索有什么不同：查询对象、排序目标、返回摘要、完整内容加载和上下文成本。
7. 失败模式：把偶然成功提炼成错误 Skill、技能过宽、版本过时、资源缺失、权限泄漏、注入过多、执行结果无人验证。
8. 如何评价一个 Skill 系统：触发准确率、任务成功率、复用次数、节省 turns/token、版本回退、错误传播范围和人工审核成本。

核心源码规则：
- 先用生命周期和状态转换讲解，再选择一条最能解释 Skill 独特性的调用链。
- 通常精读不超过 2 个文件；其余只给入口锚点。
- 可调查但不要全部展开：
  - MemoryCore/src/core/skill/skill-extractor.ts
  - MemoryCore/src/core/skill/skill-core.ts
  - MemoryCore/src/core/skill/skill-versioning.ts
  - MemoryCore/src/core/skill/skill-permission.ts
  - MemoryCore/src/core/skill/conversation-add/add-handler.ts
  - MemoryCore/src/core/skill/conversation-add/extract-worker.ts
- 阅读前说明“不看这段会误解什么”，先给伪代码，再给绝对路径、函数名、行号和局部源码。

官方资料与比较：
- 最多 3 份必读，其他为选读/查阅；精确到章节并写明读后问题。
- 项目官方资料优先；Hermes 官方 Skill 资料可作为来源与差异背景：
  - https://hermes-agent.nousresearch.com/docs/user-guide/features/skills/
  - https://hermes-agent.nousresearch.com/docs/guides/work-with-skills/
- 查证 Letta Skills、Mem0 memory、普通工具调用/Workflow 的官方资料，比较“保存知识”“保存事实”“保存可执行方法”。
- 项目复用了或受启发于其他项目，不等于行为完全相同；差异必须回到本地源码或官方文档确认。

教学和证据：
- 每次只推进一个 30～45 分钟小节；让我判断给定材料应成为 Memory、Skill、Tool 还是不保存。
- 你提供 Skill 生命周期图和跨模块时序，不要求我画。
- 使用 [项目文档] [源码确认] [测试确认] [竞品官方文档] [推断] 标签。

阶段验收：
- 我能完整解释 Skill 生命周期和治理链路。
- 我能清楚区分 Memory、Skill、Tool、Prompt、Workflow。
- 我能分析至少 5 种 Skill 失败模式及控制措施。
- 我能为另一个 Agent 项目设计一个最小 Skill Memory 方案，并说明哪些复杂能力暂时不迁移。
- 我能回答“为什么把 SOP 存成文本还不够”的面试追问。

验收后更新累积学习文档。现在先复盘已有状态，给出本阶段目录、贯穿案例和第一小节。
```

---

## Prompt 6：Wiki、CodeGraph 与生产可靠性边界

```text
你是我的开源项目导师。请带我完成 TencentDB-Agent-Memory 七阶段学习的第 6 阶段：Wiki、CodeGraph，以及支撑核心记忆系统的可靠性边界。本阶段对 Wiki/CodeGraph 讲清设计与主流程，不要求像 L0～L3 和 Skill Memory 那样深入全部实现；可靠性只聚焦会改变系统语义的机制。

仓库根目录：
C:\应用\文档\agent\memory\TencentDB-Agent-Memory

累积学习文档：
C:\应用\文档\agent\memory\TencentDB-Agent-Memory\learning\TencentDB-Agent-Memory-mental-model.md

不执行安装部署，不看前端、普通 API 或无关基础设施代码，不修改项目源码。

开始前读取累积学习文档，复盘四类资产、Fixed Binding/ACL、写入流水线、召回与 Skill。只读确认当前分支和 commit。阅读 README_CN.md、MemoryCore/README_CN.md、MemoryKnowledge/README.md 中职责和流程相关部分。

本阶段分为两条主线，但一次只讲一条。

主线 A：Wiki 与 CodeGraph
1. 为什么文档/代码知识不应简单混进 Chat Memory；四类资产的时间尺度、更新方式和检索接口如何不同。
2. MemoryCore 为什么只保存 Knowledge 元信息，而 MemoryKnowledge 负责内容 ingest、解析、索引和检索。
3. Wiki 从 source 获取、切分/组织、LLM 生成结构化页面、链接图谱、索引、ready 状态到 graph search 的主流程。
4. CodeGraph 如何表示文件、符号、调用/依赖和影响路径；它比“代码切片向量检索”多回答了什么，又不能回答什么。
5. /v3/tools/list → /v3/tools/call 的两步自发现模型为何优于把整库内容塞入 Prompt；Fixed Binding/ACL 如何限制可发现和可调用的知识资产。
6. 异步构建、自动同步、白名单/源地址校验、索引过时和构建失败如何影响使用者。

主线 B：可靠性与可运维性
1. SQLite/本地存储与 TCVDB/远端存储的抽象边界；不要把“存在后端适配器”说成“所有语义完全一致”。
2. 队列、定时器、Worker、cursor/checkpoint、幂等、去重、原子写、重试和恢复分别解决什么问题；逐项标明当前仓库是否有证据。
3. 写入成功但加工失败、L1 成功但 L2/L3 延迟、索引 ready 但内容过时、召回部分失败等“部分成功”应如何暴露给 Agent 应用。
4. 生成日志、Prompt ID/version/hash、输入输出引用、指标和 trace 如何帮助定位“记忆为什么错了”。
5. 面向多 Agent 团队时，缓存、队列、任务 key 和重试必须携带哪些隔离维度。

源码规则：
- 本阶段默认以文档、数据流和状态机为主。
- 若文档不足，只选择一条最关键链路、通常不超过 2 个文件；先说明阅读必要性。
- Wiki/CodeGraph 候选：MemoryKnowledge/src/routes/tools.ts、MemoryKnowledge/src/engines/wiki/graph-search.ts、MemoryKnowledge/src/store/build-queue.ts、MemoryKnowledge/src/store/code-graph-service.ts。
- 可靠性候选：MemoryCore/src/services/pipeline-worker.ts、MemoryCore/src/services/timer-scanner.ts、MemoryCore/src/utils/checkpoint.ts。
- 只讲关键输入、状态转换、失败输出和恢复语义，提供绝对路径、函数名和行号。

官方资料与比较：
- 最多 3 份必读，精确到章节；其余为选读/查阅。
- 项目官方文档优先；Wiki 思想可追溯项目 README 致谢中的 Karpathy LLM Wiki；CodeGraph 的复用关系应查项目 README 和上游官方仓库。
- 用 Zep/Graphiti 官方资料理解 temporal knowledge graph，但不要把它等同于本项目 Wiki/CodeGraph：https://help.getzep.com/graphiti/getting-started/welcome
- 与普通文档 RAG、代码向量检索、知识图谱/Graph RAG 做条件化比较。

教学、证据和验收：
- 30～45 分钟一节；先完成主线 A 的理解和复述，再进入主线 B。
- 你提供知识构建/调用时序和失败状态表，不要求我画图。
- 使用 [项目文档] [源码确认] [测试确认] [竞品官方文档] [推断]。
- 我最终应能：解释 Chat Memory/Wiki/CodeGraph 的边界；口述 Knowledge 构建与按需调用流程；分析至少 5 种部分失败；指出仓库可靠性证据与未知项；为另一个 Agent 项目决定何时只用 RAG、何时需要 Wiki/CodeGraph。

验收后更新累积学习文档。现在先复盘状态，给出两条主线的目录和主线 A 第一小节。
```

---

## Prompt 7：综合迁移设计与秋招模拟面试

```text
你是我的开源项目导师兼 Agent 应用开发岗位面试官。请带我完成 TencentDB-Agent-Memory 七阶段学习的第 7 阶段：证据审计、系统对比、迁移设计和模拟面试。

仓库根目录：
C:\应用\文档\agent\memory\TencentDB-Agent-Memory

累积学习文档：
C:\应用\文档\agent\memory\TencentDB-Agent-Memory\learning\TencentDB-Agent-Memory-mental-model.md

本阶段不再扩张新的知识面，目标是发现我是否真正理解，并形成可以用于秋招表达和其他 Agent/VLA 项目的设计能力。不执行安装部署，不修改源码，不做前端/API 导读。

开始前：
1. 完整读取累积学习文档，列出已确认结论、待验证问题和证据薄弱点。
2. 只读确认本地分支、commit 和文档版本。
3. 重新核对项目 README_CN.md、MemoryCore/README_CN.md、CHANGELOG.md、ROADMAP_CN.md；区分当前实现、Beta/路线图和宣传性表述。
4. 浏览 Mem0、Zep、Letta 当前官方文档，注明访问日期、产品版本、开源版/云版，避免沿用过时比较。

本阶段按以下顺序互动执行，不要一次性给出全部答案。

第一部分：证据审计
- 检查心智模型中的每个关键结论能否归入 [项目文档] [源码确认] [测试确认] [竞品官方文档] [推断]。
- 特别检查：“代码存在”是否真的接入主路径、是否默认开启、是否有测试、是否适用于当前分支。
- 对“更好用”“更准确”“更可靠”等结论，如果没有匹配协议的 benchmark 或测试，只能改写为“架构优势假设”“适用场景优势”或“待验证”。

第二部分：完整系统复述
- 让我不看资料讲清：问题空间、模块边界、四类资产、身份/隔离、L0→L3 写入流水线、召回/注入、Skill 生命周期、Wiki/CodeGraph、可靠性和失败暴露。
- 你根据我的复述逐项追问，先让我修正，再给标准化总结。
- 你可以给出最终全景图和两条主时序，但不要求我画图。

第三部分：竞品与技术对比
形成一张有条件、有证据的对比表，至少覆盖 TencentDB-Agent-Memory、普通 RAG、Mem0、Zep、Letta，比较：
- 核心记忆单位与抽象层次；
- 谁触发写入、谁决定更新；
- 冲突、过时和可追溯性；
- 关键词/向量/混合/图检索与 rerank；
- Prompt 注入、工具下钻和上下文预算；
- user/session/agent/team 隔离与共享治理；
- 程序性记忆/Skill；
- 文档和代码知识资产；
- 可观测性、失败恢复、部署依赖与成熟度；
- 最适用和不适用的场景。
每个维度都要回答“为什么”，不能只打勾。云版和开源版分开比较。

官方资料最多给 3 份本阶段必读，其他资料只做查阅。官方候选入口：
- https://github.com/TencentCloud/TencentDB-Agent-Memory
- https://docs.mem0.ai/open-source/overview
- https://help.getzep.com/v2/memory
- https://help.getzep.com/v2/understanding-the-graph
- https://docs.letta.com/
所有链接要先在线核验，说明具体章节、版本和读后问题。

第四部分：迁移设计终考
先让我选择或确认一个具体 Agent/VLA 场景。然后要求我独立设计一套记忆系统，至少说明：
- 要保存和明确不保存的信息；
- 记忆层次和数据模型；
- 写入触发、抽取、更新、遗忘/纠错；
- 检索、排序、上下文预算和注入；
- 权限、租户/Agent 隔离；
- 是否需要 Skill、Wiki、CodeGraph；
- 队列、幂等、失败恢复和可观测性；
- 评测集、指标和对照实验；
- 从 TencentDB-Agent-Memory 迁移哪些思想、删掉哪些复杂性、为什么。
你只能先提问和指出缺口，不要立即替我完成设计。等我提交后，再做设计评审。

第五部分：模拟面试
- 进行至少 20 分钟强度的 Agent 应用开发岗位面试，一次问一个问题，根据回答继续追问。
- 必须覆盖：为什么普通 RAG 不够、L0～L3 的价值和风险、hybrid/RRF、记忆污染、隔离、Skill、竞品对比、可靠性、评测和迁移取舍。
- 不接受口号式答案；持续追问证据、反例、适用条件和简化方案。
- 面试结束给出“正确性、系统性、取舍意识、证据意识、表达清晰度”五项反馈和针对性补课清单。

源码规则：
- 本阶段不主动增加源码阅读。
- 只有证据审计发现某个核心结论无法确认时，才回到此前已经定位的关键函数，使用局部源码验证；说明为什么需要回看。

最终验收和文档：
- 我能脱离文档完整复述系统；
- 我能对普通 RAG、Mem0、Zep、Letta 做有条件、有证据的比较；
- 我能完成一个可落地但不过度设计的迁移方案；
- 我能面对连续追问并主动说明未知项；
- 把最终确认的全景、流程、对比表、迁移设计、面试薄弱点和后续学习项写回累积学习文档。

现在只执行第一部分“证据审计”：先给审计方法和发现的问题，不要提前进入迁移设计或模拟面试。
```

---

## 官方资料入口说明

这些入口用于给七条 Prompt 提供起点，不代替执行阶段的实时核验：

- TencentDB-Agent-Memory 官方仓库：https://github.com/TencentCloud/TencentDB-Agent-Memory
- Mem0 Open Source Overview：https://docs.mem0.ai/open-source/overview
- Mem0 Open Source Features：https://docs.mem0.ai/open-source/features/overview
- Zep Memory：https://help.getzep.com/v2/memory
- Zep Graph Concepts：https://help.getzep.com/v2/understanding-the-graph
- Graphiti Overview：https://help.getzep.com/graphiti/getting-started/welcome
- Letta Docs：https://docs.letta.com/
- Elasticsearch RRF：https://www.elastic.co/docs/reference/elasticsearch/rest-apis/reciprocal-rank-fusion
- Hermes Skills：https://hermes-agent.nousresearch.com/docs/user-guide/features/skills/

外部系统更新较快。执行每日 Prompt 时，应重新确认文档日期、开源版/云版以及与本地仓库 `feat/server_team` 分支的差异。
