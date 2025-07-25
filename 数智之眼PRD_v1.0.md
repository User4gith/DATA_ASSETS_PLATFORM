### **“数智之眼” — 企业内部数据资产管理终端产品设计方案 (PRD)**

**版本**: V1.0
**创建日期**: 2025/06/05

---

### 1. 项目概述

#### 1.1 项目背景与愿景
随着公司业务的快速发展和数字化转型的深入，数据已成为公司的核心战略资产。然而，数据资产分散在各个业务系统、数据库和数据仓库中，存在着“找数难、信数难、用数难”的普遍问题。为了解决这些痛点，我们计划设计并开发一个统一的、可视化的Web端数据终端——“数智之眼”。

**项目愿景**：打造一个集“看、管、统、报、警”于一体的一站式数据资产管理与服务平台，让数据可见、可懂、可用、可控，最终赋能业务决策，提升数据驱动能力。

#### 1.2 目标用户
本产品的用户群体根据其对数据的需求和操作权限，主要分为以下几类：

*   **数据分析师/业务人员**：主要用户。需要快速查找、理解和使用数据，制作报表以支持业务决策。
*   **数据工程师/开发人员**：负责数据的生产和维护。需要管理数据资产元信息、监控数据任务、处理数据质量问题。
*   **数据管理/治理团队**：负责制定数据标准和规范。需要对数据资产进行盘点、分类、定级和审计。
*   **企业管理层**：需要通过宏观的统计和报表，了解公司整体的数据资产状况和价值。

#### 1.3 核心价值
*   **提升效率**：通过统一的搜索和目录，将找数据的时间从数小时/天缩短到分钟级。
*   **提高质量**：通过数据质量监控和报警，主动发现并解决数据问题，提升数据可信度。
*   **保障安全**：通过精细化的权限管控和审计，确保数据在合规的前提下被访问和使用。
*   **释放价值**：通过便捷的报表和分析工具，降低数据使用门槛，让人人都是数据分析师。

---

### 2. 产品整体架构

#### 2.1 信息架构 (IA)
产品的顶层导航将围绕用户的核心任务流展开，主要分为以下几个模块：

*   **首页仪表盘 (Dashboard)**：用户登录后的第一入口，提供个性化的概览和快捷入口。
*   **数据资产中心 (Data Asset Center)**：核心模块，提供数据资产的“看”和“管”。
*   **数据统计与报表 (Statistics & Reporting)**：提供数据资产的“统”和“报”。
*   **监控与告警 (Monitoring & Alerting)**：提供数据资产的“警”。
*   **系统管理 (System Management)**：为管理员提供后台配置功能。

#### 2.2 技术架构简述 (可选)
*   **前端**：建议采用 Vue 3 / React 18 + TypeScript + Ant Design / Element Plus，实现响应式和组件化开发。
*   **后端**：建议采用 Java (Spring Boot) / Python (Django/FastAPI) / Go，提供稳定的RESTful API。
*   **数据存储**：
    *   元数据存储：MySQL / PostgreSQL。
    *   搜索引擎：Elasticsearch，用于全文检索。
    *   数据血缘/图谱：Neo4j 等图数据库。
*   **集成**：通过API或JDBC/ODBC连接到公司现有的数据仓库（如Hive, ClickHouse）、数据库（MySQL, Oracle）、BI系统（Tableau, PowerBI）等。

---

### 3. 核心功能模块详细设计

#### 3.1 首页仪表盘 (Dashboard)
*   **核心目标**：为不同角色的用户提供个性化的工作台，展示其最关心的信息。
*   **功能点**：
    1.  **核心指标卡片**：展示全公司数据资产总数、昨日新增、核心数据表数量、数据质量平均分等KPI。
    2.  **我的待办 (To-Do List)**：显示待我审批的数据权限申请、待处理的数据质量告警等。
    3.  **我的收藏/最近访问**：快捷访问用户收藏的数据表、报表或仪表盘。
    4.  **平台公告**：发布平台更新、数据规范、重要通知等。
    5.  **热门资产榜单**：展示全公司访问最频繁、评价最高的数据表或数据集。

#### 3.2 数据资产中心 (Data Asset Center)
*   **核心目标**：实现数据资产的全面、准确、统一的视图和管理。
*   **子模块**：
    1.  **资产地图 (Asset Catalog)**
        *   **统一搜索框**：支持按表名、字段名、业务描述、标签等进行全文模糊搜索，并提供高级筛选（如按数据源、负责人、创建时间等）。
        *   **资产目录树**：按“数据源 -> 数据库 -> 数据表”或“业务域 -> 业务过程 -> 数据表”的层级结构展示，方便用户浏览。
        *   **资产列表**：展示搜索或浏览结果，包含关键信息（名称、类型、负责人、描述、热度、质量分）。

    2.  **资产详情页 (Asset Detail Page)** - **这是“看”的核心**
        *   **基础信息**：表名、数据源、负责人、创建/更新时间、生命周期状态（在线、下线、归档）。
        *   **业务元数据**：业务定义、业务口径、所属业务域、标签（Tags，如`核心表`, `用户行为`, `财务数据`）。用户可申请编辑。
        *   **技术元数据**：表结构（字段名、类型、是否主键、注释）、分区信息、存储大小。
        *   **数据预览**：展示前100行抽样数据（需做脱敏处理）。
        *   **数据血缘 (Data Lineage)**：**可视化图谱**，清晰展示该表的上游来源和下游影响，帮助用户追溯数据源头和评估变更影响。
        *   **数据质量**：展示该表的最新质量评估报告链接或核心质量指标（如完整性、唯一性、及时性）。
        *   **使用统计**：展示该表近30天的访问次数、Top 10访问用户等。
        *   **权限信息**：展示当前用户的权限，并提供“申请权限”入口。

    3.  **资产管理 (Asset Management)** - **这是“管”的核心**
        *   **元数据编辑**：允许有权限的用户（如数据负责人）编辑业务元数据和标签。
        *   **生命周期管理**：提供数据表的“上线”、“下线”、“归档”申请和审批流程。
        *   **标签管理**：管理员可以创建、修改、删除全局标签体系。

#### 3.3 数据统计与报表 (Statistics & Reporting)
*   **核心目标**：提供宏观洞察和自助式报表能力。
*   **子模块**：
    1.  **资产统计大盘**
        *   提供多维度的统计图表，如：按数据源分布、按业务域分布、按数据层级（ODS, DWD, DWS, ADS）分布、资产总数增长趋势图等。
    2.  **自定义报表工具**
        *   允许用户通过**拖拽**的方式，选择已获权限的数据表，配置维度和指标，生成自定义报表。
        *   支持图表类型切换（表格、折线图、柱状图、饼图）。
        *   支持报表的保存、分享和导出（Excel, PDF）。
    3.  **预设模板报表**
        *   内置常用的管理报表，如《周度数据质量报告》、《核心业务数据增长报告》等，可订阅并定时发送（如邮件）。

#### 3.4 监控与告警 (Monitoring & Alerting)
*   **核心目标**：主动发现数据问题，保障数据稳定可靠。
*   **子模块**：
    1.  **监控规则配置**
        *   允许数据负责人或工程师为数据表配置监控规则。
        *   **规则类型**：
            *   **及时性规则**：例如，`每日分区产出时间 < 08:00:00`。
            *   **完整性规则**：例如，`核心字段非空率 > 99%`。
            *   **唯一性规则**：例如，`主键唯一`。
            *   **波动性规则**：例如，`日增行数波动范围 < ±30%`。
    2.  **告警中心**
        *   集中展示所有触发的告警信息，包括告警对象、规则、时间、状态（待处理、处理中、已解决）。
        *   支持筛选和排序。
        *   点击告警可查看详情并进行处理（如：指派、标记解决、忽略）。
    3.  **通知配置**
        *   支持配置告警的通知方式（邮件、企业微信/钉钉、短信）和通知人（数据负责人、自定义接收人组）。

#### 3.5 系统管理 (System Management)
*   **核心目标**：为平台管理员提供后台管理能力。
*   **子模块**：
    1.  **用户与角色管理**：管理平台用户，并为用户分配角色（如前述定义的管理员、分析师等）。
    2.  **权限管理**：配置不同角色的功能菜单权限和默认数据权限策略。
    3.  **数据源管理**：配置和管理平台需要接入的各类数据源的连接信息。
    4.  **操作日志**：记录所有用户的关键操作，用于审计和问题追溯。

---

### 4. 权限设计 (RBAC)

采用基于角色的访问控制（Role-Based Access Control）模型。

| 角色 | 首页仪表盘 | 数据资产中心 | 统计与报表 | 监控与告警 | 系统管理 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **企业管理层** | 查看 | 仅查看（高层级数据） | 查看预设报表 | 查看宏观告警 | 无 |
| **数据分析师** | 查看个人视图 | 查看、搜索、申请权限、收藏 | 查看、创建自定义报表 | 查看已发布告警 | 无 |
| **数据工程师** | 查看个人视图 | 完全访问（负责的资产），管理元数据、血缘 | 查看、创建 | 配置规则、处理告警 | 无 |
| **数据治理团队** | 查看 | 完全访问，管理标签体系，审计 | 查看所有报表 | 查看所有告警 | 无 |
| **超级管理员** | 查看 | 完全访问 | 完全访问 | 完全访问 | **完全控制** |

*注：数据权限（即用户能看到哪些表的数据）将与公司现有的数据安全体系结合，可按“用户-角色-数据”或更精细的“用户-数据”进行控制。*

---

### 5. 产品路线图 (Roadmap)

建议分阶段实施，以快速验证价值并收集反馈。

*   **Phase 1: MVP (Minimum Viable Product) - 预计3个月**
    *   **核心功能**：打通1-2个核心数据源，实现数据资产的搜索、查看（基础信息、技术元数据、血缘）、数据预览。
    *   **目标**：解决“找数难”和“懂数难”的核心问题。

*   **Phase 2: 增强版 - 预计后续3个月**
    *   **核心功能**：完善资产管理流程（元数据编辑、生命周期）、上线数据质量监控与告警、推出资产统计大盘。
    *   **目标**：解决“信数难”和“管数难”的问题。

*   **Phase 3: 智能化平台 - 预计后续6个月**
    *   **核心功能**：上线自定义报表工具、集成AI能力（如：智能数据推荐、自然语言查询-Text2SQL）、完善数据治理能力（如：敏感数据识别）。
    *   **目标**：降低数据使用门槛，向“人人都是数据分析师”迈进。

---

### 6. 总结

“数智之眼”数据终端旨在通过系统化的方式，将公司的数据资产管理起来、利用起来。本方案从用户需求出发，设计了完整的产品功能和架构。项目的成功将依赖于跨部门的紧密协作、敏捷的开发迭代以及对用户反馈的持续关注。我们相信，该平台将成为公司数字化转型道路上的关键基础设施。