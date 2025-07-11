

### **“数智之眼” — 企业内部数据资产管理终端产品需求文档 (PRD)**

**版本**: V2.0 (正式版)
**创建日期**: 2025/06/12
**修订日期**: 2025/06/12

---

#### **0. 文档修订历史**

| 版本号 | 修订日期 | 修订人 | 修订内容 |
| :--- | :--- | :--- | :--- |
| V1.0 | 2025/06/12 | AI助手 | 初稿创建，包含基本框架和功能点。 |
| V1.1 | 2025/06/12 | AI助手 | 补充范围、指标、非功能需求；深化部分功能描述。 |
| **V2.0** | **2025/06/12** | **AI助手** | **最终版。以“企业管理人员”为核心视角，对所有功能模块进行了详尽描述，包含用户故事、业务规则、逻辑流程、输入输出和异常状态。** |

---

### **1. 项目概述**

#### 1.1 项目背景与愿景
（内容不变）随着公司业务的快速发展...我们计划设计并开发一个统一的、可视化的Web端数据终端——“数智之眼”。
**项目愿景**：打造一个集“看、管、统、报、警”于一体的一站式数据资产管理与服务平台，让数据可见、可懂、可用、可控，最终赋能业务决策，提升数据驱动能力。

#### 1.2 目标用户
*   **企业管理层 (核心用户)**：需要通过宏观的统计和报表，了解公司整体的数据资产健康度、价值分布和安全风险，以支持战略决策和评估数据战略的ROI。
*   **数据分析师/业务人员**：需要快速查找、理解和使用数据，制作报表以支持业务决策。
*   **数据工程师/开发人员**：负责数据的生产和维护。需要管理数据资产元信息、监控数据任务、处理数据质量问题。
*   **数据管理/治理团队**：负责制定数据标准和规范。需要对数据资产进行盘点、分类、定级和审计。

#### 1.3 项目范围 (Scope) - MVP阶段
（内容不变）

#### 1.4 衡量指标 (Success Metrics)
（内容不变）

---

### **2. 功能性需求 (Functional Requirements)**

#### 2.1 信息架构 (IA)
（内容不变）

#### 2.2 核心功能模块详细描述

##### **2.2.1 模块一：首页仪表盘 (Dashboard)**

*   **功能概述**：为管理层提供一个全局、实时的数据资产健康度与价值快照，作为其感知公司数据脉搏的“驾驶舱”。

*   **PBI-101: 全局资产与质量概览**
    *   **用户故事**: 作为一个**企业管理者**，我希望一登录就能看到公司数据资产的总量、核心资产占比以及整体的数据质量趋势，这样我能快速判断我们的数据基础是否稳固，是否在向好的方向发展。
    *   **业务规则**:
        1.  所有指标均为全公司范围的聚合数据。
        2.  “数据质量分”采用加权平均计算，核心资产的权重更高。
        3.  页面数据默认每小时自动刷新一次。
    *   **逻辑流程**: 用户登录 -> 系统识别为“管理者”角色 -> API请求聚合指标数据 -> 前端渲染“核心指标卡”和“数据质量概览”组件。
    *   **输入/输出**:
        *   **输入**: 用户登录凭证。
        *   **输出**: 展示以下指标的卡片：资产总数、核心资产数、数据质量平均分、昨日告警数。
    *   **异常状态**:
        *   **数据加载失败**: 对应卡片显示“数据加载失败，请稍后重试”的提示，并提供手动刷新按钮。
        *   **首次使用无数据**: 显示“系统正在初始化统计，请稍后查看”。

*   **PBI-102: 热门高价值资产洞察**
    *   **用户故事**: 作为一个**企业管理者**，我想知道公司里哪些数据是最被频繁使用的“明星数据”，这有助于我了解业务焦点在哪里，并确保这些高价值资产得到重点保护和投入。
    *   **业务规则**:
        1.  热度排名基于近30天内资产的访问次数、查询次数、被下游引用次数综合计算。
        2.  榜单只显示TOP 10的数据资产。
        3.  管理者点击榜单项可跳转查看资产详情（只读）。
    *   **逻辑流程**: 每日定时任务计算所有资产的热度得分并缓存 -> 用户访问首页 -> API读取缓存的热度榜单数据 -> 前端渲染“热门资产榜”组件。
    *   **输入/输出**:
        *   **输入**: 无。
        *   **输出**: 展示TOP 10热门资产的列表，包含排名、资产名称、所属业务域。
    *   **异常状态**:
        *   **榜单数据未生成**: 组件显示“热门数据正在计算中...”。

---

##### **2.2.2 模块二：数据资产中心 (Data Asset Center)**

*   **功能概述**：为管理层提供一个透明的、可追溯的、统一的数据资产视图，以审查和理解关键业务指标背后的数据实体。

*   **PBI-201: 业务视角的数据资产搜索与浏览**
    *   **用户故事**: 作为一个**企业管理者**，当我在会议上听到一个关键业务术语（如“月活跃用户MAU”）时，我希望能立刻在系统里搜到它对应的数据表，查看它的权威业务口径和负责人，以确保我们讨论的是基于同一事实。
    *   **业务规则**:
        1.  搜索不仅匹配表名、字段名，还必须支持匹配业务描述和标签。
        2.  搜索结果对管理者默认隐藏技术细节（如分区信息），优先展示业务元数据。
        3.  资产目录树可以按“业务域”进行组织，方便管理者从自己熟悉的业务板块进行浏览。
    *   **逻辑流程**: 用户在搜索框输入关键字 -> 后端调用Elasticsearch进行全文检索 -> 返回结果列表 -> 用户点击结果项进入详情页。
    *   **输入/输出**:
        *   **输入**: 搜索关键词、筛选条件（如业务域）。
        *   **输出**: 匹配的数据资产列表，包含名称、描述、负责人、热度。
    *   **异常状态**:
        *   **无搜索结果**: 页面提示“未找到相关数据资产，请尝试其他关键词”。
        *   **搜索服务超时**: 页面提示“搜索服务繁忙，请稍后重试”。

*   **PBI-202: 资产详情页（管理者视角）**
    *   **用户故事**: 作为一个**企业管理者**，在查看一个核心数据资产（如“用户订单表”）的详情时，我最关心的是三件事：它的业务定义是什么？它由谁负责？如果它出了问题，会影响到哪些下游的关键报表（比如我的财务报表）？
    *   **业务规则**:
        1.  详情页对管理者为**只读**模式，不可编辑。
        2.  **数据预览**功能对管理者默认关闭或只显示脱敏后的数据摘要，防止敏感数据泄露。
        3.  **数据血缘**图谱必须清晰、可视化地展示上下游影响，这是管理层决策的关键依据。
    *   **逻辑流程**: 用户进入详情页 -> 并发请求基础信息、业务元数据、血缘关系、质量报告等API -> 数据返回后，前端组合渲染页面 -> 血缘图使用专用图形库绘制。
    *   **输入/输出**:
        *   **输入**: 数据资产的唯一ID。
        *   **输出**: 一个完整的详情页面，重点突出：
            *   **业务元数据**：业务定义、业务口径、负责人、所属业务域。
            *   **数据血缘图**：可视化的上下游依赖关系。
            *   **数据质量摘要**：当前的质量得分和主要问题。
    *   **异常状态**:
        *   **资产不存在**: 显示404页面。
        *   **血缘图数据加载失败**: 在血缘图区域显示“血缘关系加载失败”。
        *   **无权访问**: 显示“您没有查看此资产的权限”，并提供资产负责人的联系方式。

---

##### **2.2.3 模块三：数据统计与报表 (Statistics & Reporting)**

*   **功能概述**：为管理层提供预设的、宏观的、趋势性的数据治理报告，用于评估数据战略的执行效果。

*   **PBI-301: 数据资产统计大盘**
    *   **用户故事**: 作为一个**企业管理者**，我需要一个全局的统计大盘，通过图表直观地看到公司的数据资产是如何按业务域、按重要等级分布的，以及资产总量的增长趋势，这能帮助我进行资源规划和预算分配。
    *   **业务规则**:
        1.  所有图表均为只读，不可进行下钻和修改。
        2.  数据每日更新一次。
        3.  提供报表导出为PDF的功能，方便用于线下汇报。
    *   **逻辑流程**: 用户进入统计大盘页面 -> API请求各项预聚合的统计数据 -> 前端使用ECharts等图表库进行渲染。
    *   **输入/输出**:
        *   **输入**: 无。
        *   **输出**: 包含以下图表的仪表盘页面：
            *   按业务域分布的资产数量（饼图）。
            *   按数据层级分布的资产数量（柱状图）。
            *   近6个月资产总数增长趋势（折线图）。
            *   核心资产与非核心资产占比（环形图）。
    *   **异常状态**:
        *   **报表数据计算中**: 页面显示“昨日数据正在统计中，请于XX时间后查看”。

---

##### **2.2.4 模块四：监控与告警 (Monitoring & Alerting)**

*   **功能概述**：让管理层能够感知到影响业务的重大数据事件，并追溯问题责任人。

*   **PBI-401: 核心资产告警看板**
    *   **用户故事**: 作为一个**企业管理者**，我不需要关心零星的数据问题，但我必须第一时间知道公司的核心数据资产（如财务、用户核心表）是否发生了严重的数据质量事故，并能看到该事故的责任人和处理状态。
    *   **业务规则**:
        1.  该看板只显示“严重”级别且发生在“核心资产”上的告警。
        2.  信息以列表形式展示，包含：告警时间、资产名称、问题描述、当前状态（处理中/已解决）、负责人。
        3.  管理者不能处理告警，但可以点击查看告警详情。
    *   **逻辑流程**: 用户进入告警看板 -> API请求符合筛选条件（严重、核心）的告警列表 -> 前端渲染列表。
    *   **输入/输出**:
        *   **输入**: 无。
        *   **输出**: 一个过滤后的严重告警列表。
    *   **异常状态**:
        *   **当前无严重告警**: 页面显示“一切正常，当前无严重数据质量事件”。

---

##### **2.2.5 模块五：系统管理 (System Management)**

*   **功能概述**：确保平台本身的安全、合规与可审计，满足管理层的风控要求。

*   **PBI-501: 审计日志（管理者视角）**
    *   **用户故事**: 作为一个**企业管理者**，为了确保数据安全合规，我需要知道系统能够记录所有对核心敏感数据的访问操作，并且这些日志是可追溯、不可篡改的，以备审计之需。
    *   **业务规则**:
        1.  管理者本身不能直接访问和修改操作日志。
        2.  此功能主要由“系统管理员”或“审计员”角色使用。
        3.  PRD中描述此功能的存在性，是为了向管理层证明平台的安全性。
    *   **逻辑流程 (对管理员而言)**: 管理员进入审计日志页面 -> 可按用户、操作类型、时间范围进行筛选 -> 查看详细的操作记录。
    *   **输入/输出 (对管理员而言)**:
        *   **输入**: 筛选条件。
        *   **输出**: 符合条件的操作日志列表，包含时间、用户、IP地址、操作对象、操作内容。
    *   **异常状态**:
        *   **日志查询超时**: 提示“查询超时，请缩小时间范围重试”。

---

### **3. 非功能性需求 (Non-Functional Requirements)**
（内容不变）

### **4. 术语表 (Glossary)**
（内容不变）

### **5. 产品路线图 (Roadmap)**
（内容不变）