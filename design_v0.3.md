# Advanced-get-biji-Skills 开发需求

> **版本历史**
> - v0.1: 初始版本 (design_v0.1.md)
> - v0.2: 增强MCP工具集成，优化路由策略 (design_v0.2.md)
> - v0.3: 精简文档，MCP配置外置引用 (当前版本)

> **MCP配置文件**: `/Users/qinxiaoqiang/Downloads/advance_get_biji_skills/mcp_toolkits.json`

---

## 1. 项目目标与定位

**项目名称**：Advanced-get-biji-Skills（Get笔记智能技能系统专业版）

**核心目标**：  
在已有「get 笔记」MCP 的基础上，叠加多源搜索和LLM知识能力，依据SCAN框架，构建一个多角色协同的 Skills 体系，让 AI 能够围绕用户目标（如示例：  
>「基于我的笔记，分析限流方案风险并写博客」  
完成从**研究 → 写作 → 验证**的一整套高质量工作流，而不是单纯"帮忙查笔记"和"基于MCP的检索输出"，能够稳定、全面和客观的保留证据，提供深度报告，并保留信源、标注分歧点作为高价值输出。

---

## 2. MCP 工具集架构（v0.2 更新）

### 2.1 MCP 工具分类与优先级

```
┌─────────────────────────────────────────────────────────────────┐
│                      MCP 工具调用层                              │
├─────────────────────────────────────────────────────────────────┤
│  ★★★ 核心必选（每次研究任务必须调用）                            │
│  ├─ get-notes-multi-kbs  → 个人知识库/笔记检索（第一信源）        │
│  ├─ metaso               → AI智能搜索（综合信息聚合）             │
│  └─ pubmed-data-server   → PubMed医学文献检索                   │
├─────────────────────────────────────────────────────────────────┤
│  ★★ 医学增强（医学类问题必选）                                   │
│  ├─ knows-mcp            → 医学指南/论文/会议证据                │
│  └─ mcp-pubmed-llm-server → PubMed LLM增强检索                  │
├─────────────────────────────────────────────────────────────────┤
│  ★ 辅助可选（按需调用）                                          │
│  ├─ Tavily               → 通用网页搜索（技术/新闻类）            │
│  ├─ bingcn               → 必应中文搜索（中文资讯）               │
│  ├─ ollama_web_search    → 本地Ollama搜索                       │
│  └─ time                 → 时间服务                              │
├─────────────────────────────────────────────────────────────────┤
│  🧠 思维辅助                                                     │
│  └─ sequential-thinking  → 复杂问题分步推理                      │
└─────────────────────────────────────────────────────────────────┘
```

### 2.2 各MCP工具详细说明

| MCP名称 | 用途 | 调用时机 | 返回内容 |
|---------|------|----------|----------|
| **get-notes-multi-kbs** | 个人笔记/知识库检索 | 任何研究任务的第一步 | 笔记元数据、全文内容 |
| **metaso** | AI智能搜索引擎 | 需要综合搜索时（替代传统web_search） | AI整理后的结构化搜索结果 |
| **pubmed-data-server** | PubMed医学文献深度检索 | 医学研究、临床证据查找 | 摘要、全文、文献元数据 |
| **knows-mcp** | 医学指南/论文/会议资料 | 需要权威医学证据时 | 指南、论文、会议摘要 |
| **mcp-pubmed-llm-server** | PubMed LLM增强 | 复杂医学问题分析 | LLM分析后的文献洞察 |
| **Tavily** | 通用网页搜索 | 技术博客、新闻资讯 | 网页摘要、链接 |
| **bingcn** | 必应中文搜索 | 中文资讯、国内内容 | 中文网页结果 |
| **sequential-thinking** | 分步推理 | 复杂多步骤问题 | 结构化思维链 |

---

## 3. 路由层策略（v0.2 更新）

### 3.1 意图识别与MCP路由矩阵

```json
{
  "routing_rules": {
    "research": {
      "医学研究类": {
        "keywords": ["临床", "治疗", "药物", "副作用", "预后", "检查", "指标", "癌症", "肿瘤"],
        "required_mcp": ["get-notes-multi-kbs", "metaso", "pubmed-data-server", "knows-mcp"],
        "optional_mcp": ["mcp-pubmed-llm-server"]
      },
      "技术研究类": {
        "keywords": ["架构", "方案", "设计", "性能", "优化", "实现", "代码"],
        "required_mcp": ["get-notes-multi-kbs", "metaso"],
        "optional_mcp": ["Tavily", "bingcn"]
      },
      "通用研究类": {
        "keywords": ["分析", "调研", "对比", "总结"],
        "required_mcp": ["get-notes-multi-kbs", "metaso"],
        "optional_mcp": ["Tavily"]
      }
    },
    "writing": {
      "required_mcp": ["get-notes-multi-kbs"],
      "optional_mcp": ["metaso"]
    },
    "validation": {
      "医学验证": {
        "required_mcp": ["get-notes-multi-kbs", "knows-mcp", "pubmed-data-server"]
      },
      "通用验证": {
        "required_mcp": ["get-notes-multi-kbs", "metaso"]
      }
    }
  }
}
```

### 3.2 搜索策略优化

#### 3.2.1 搜索优先级

1. **个人知识优先**：`get-notes-multi-kbs` 永远第一个调用
2. **智能搜索主导**：`metaso` 作为主要外部搜索（替代传统web_search）
3. **专业文献补充**：医学类用 `pubmed-data-server` + `knows-mcp`
4. **通用搜索辅助**：`Tavily`/`bingcn` 作为补充

#### 3.2.2 并行调用策略

```
Phase 1: 侦察阶段（并行）
├─ get-notes-multi-kbs  ───┐
├─ metaso               ───┼─→ 聚合结果
└─ pubmed-data-server   ───┘   (医学类)

Phase 2: 深度获取（按需串行）
├─ knows-mcp (获取具体指南)
└─ mcp-pubmed-llm-server (LLM分析)

Phase 3: 补充验证（可选并行）
├─ Tavily (最新资讯)
└─ bingcn (中文补充)
```

---

## 4. SCAN 研究框架（v0.2 MCP整合版）

### 4.1 S：Survey/Scout（侦察）

**MCP调用策略**：

```python
# 并行调用三个核心MCP
parallel_calls = [
    {
        "mcp": "get-notes-multi-kbs",
        "action": "searchNotes",
        "params": {"keywords": extracted_keywords, "limit": 10}
    },
    {
        "mcp": "metaso",
        "action": "search",
        "params": {"query": research_topic, "mode": "deep"}
    },
    {
        "mcp": "pubmed-data-server",  # 仅医学类
        "action": "searchPubMed",
        "params": {"query": medical_keywords, "limit": 10}
    }
]
```

**输出**：
- 笔记候选列表（ID + 标题 + 摘要）
- metaso搜索结果（已AI整理）
- PubMed文献列表（医学类）

### 4.2 C：Clarify/Compare（澄清与比较）

**信息来源权重表（v0.2更新）**：

| 来源类型 | MCP | 基础权重 | 说明 |
|----------|-----|----------|------|
| 个人实践笔记 | get-notes-multi-kbs | 0.85 | 一手经验，最高信任 |
| 医学指南 | knows-mcp (GUIDE) | 0.95 | 权威指南 |
| 医学论文 | pubmed-data-server | 0.85-0.90 | 取决于期刊影响因子 |
| 会议资料 | knows-mcp (MEETING) | 0.75 | 前沿但未经验证 |
| AI智能搜索 | metaso | 0.70 | 已聚合但需验证 |
| 通用网页 | Tavily/bingcn | 0.50-0.65 | 参考价值 |
| LLM推断 | - | 0.30-0.40 | 最低权重 |

**冲突检测增强**：
```json
{
  "conflict_detection": {
    "note_vs_guideline": "当笔记与指南冲突，标记为'实践差异'",
    "guideline_vs_paper": "当指南与最新论文冲突，标记为'证据更新中'",
    "metaso_aggregation": "metaso结果作为冲突裁判参考"
  }
}
```

### 4.3 A：Analyze/Assess（分析与评估）

**深度调用策略**：

```python
# 基于C阶段发现的关键问题，深度获取证据
if topic_type == "medical":
    # 获取具体指南内容
    knows_details = call_mcp("knows-mcp", "getGuideDetails", {...})
    # LLM分析文献
    llm_analysis = call_mcp("mcp-pubmed-llm-server", "analyzeArticles", {...})
```

### 4.4 N：Navigate/Note（导航与记录）

**输出格式（增强版）**：

```markdown
## 研究报告

### 1. 背景与问题定义
...

### 2. 证据来源概览
| 来源类型 | 数量 | MCP来源 |
|----------|------|---------|
| 个人笔记 | 5 | get-notes-multi-kbs |
| 医学指南 | 2 | knows-mcp |
| PubMed文献 | 8 | pubmed-data-server |
| 综合搜索 | 3 | metaso |

### 3. 核心发现
...

### 4. 风险与分歧点
- [分歧1] 笔记观点 vs 指南建议
  - 笔记来源: Note#xxx
  - 指南来源: GUIDE#xxx
  - 建议采信: 指南（权重更高）

### 5. 推荐方案
...

### 6. 引用索引
- [Note#1] get-notes-multi-kbs: xxx
- [PubMed#1] pubmed-data-server: PMID:xxx
- [Guide#1] knows-mcp: xxx
- [Web#1] metaso: xxx

### 7. 置信度评级
整体置信度: 高/中/低
```

---

## 5. 多角色 Skills 需求（保持不变，增加MCP绑定）

### 5.1 Researcher Skill

**绑定MCP**：
- 必选：`get-notes-multi-kbs`, `metaso`
- 医学类追加：`pubmed-data-server`, `knows-mcp`
- 复杂问题：`sequential-thinking`

### 5.2 Writer Skill

**绑定MCP**：
- 必选：`get-notes-multi-kbs`（引用原始笔记内容）
- 可选：`metaso`（补充写作素材）

### 5.3 Validator Skill

**绑定MCP**：
- 必选：`get-notes-multi-kbs`, `knows-mcp`（验证依据）
- 医学验证：`pubmed-data-server`

---

## 6. 工具配置

> **配置文件路径**: `/Users/qinxiaoqiang/Downloads/advance_get_biji_skills/mcp_toolkits.json`
>
> 开发时读取该文件获取完整MCP配置参数。

### 6.1 路由关键词配置

| 类型 | 关键词 |
|------|--------|
| 医学类 | 临床、治疗、药物、副作用、预后、检查、指标、癌症、肿瘤、胰腺、化疗、放疗、免疫 |
| 技术类 | 架构、方案、设计、性能、优化、实现、代码、系统、服务 |
| 默认搜索 | metaso |

---

## 7. 实施建议（v0.2 更新）

### 7.1 第一阶段（MVP）

- [x] 定义MCP工具集
- [ ] 实现Routing Layer（基于关键词的MCP选择）
- [ ] 实现Researcher Skill + SCAN（S阶段完整并行调用）
- [ ] 集成三个核心MCP：`get-notes-multi-kbs`, `metaso`, `pubmed-data-server`

### 7.2 第二阶段

- [ ] 加入 `knows-mcp` 深度医学证据
- [ ] 实现 Writer Skill
- [ ] 实现 Validator Skill
- [ ] 冲突检测与权重融合

### 7.3 第三阶段

- [ ] 优化并行调用性能
- [ ] 添加缓存层
- [ ] 扩展更多领域的路由规则

---

## 8. 附录：MCP调用示例

### 8.1 医学研究场景

```
用户问题: "胰腺癌患者化疗后白细胞低该如何处理？"

路由决策:
- 意图: 医学研究
- 必选MCP: get-notes-multi-kbs, metaso, pubmed-data-server, knows-mcp

调用序列:
1. [并行] get-notes-multi-kbs.searchNotes("化疗 白细胞低")
2. [并行] metaso.search("胰腺癌化疗白细胞减少处理方案")
3. [并行] pubmed-data-server.search("pancreatic cancer chemotherapy leukopenia management")
4. [串行] knows-mcp.searchGuides("化疗相关性白细胞减少症")

融合输出: 结构化报告 + 证据溯源
```

### 8.2 技术研究场景

```
用户问题: "基于我的笔记，分析限流方案风险并写博客"

路由决策:
- 意图: 技术研究 + 写作
- 必选MCP: get-notes-multi-kbs, metaso
- 可选MCP: Tavily

调用序列:
1. [并行] get-notes-multi-kbs.searchNotes("限流 风险")
2. [并行] metaso.search("2024 微服务限流方案最佳实践")
3. [可选] Tavily.search("rate limiting production patterns")

融合后交给Writer Skill生成博客
```

---

**文档版本**: v0.3  
**更新日期**: 2026-01-15  
**主要变更**: 精简文档，MCP配置外置到mcp_toolkits.json引用
