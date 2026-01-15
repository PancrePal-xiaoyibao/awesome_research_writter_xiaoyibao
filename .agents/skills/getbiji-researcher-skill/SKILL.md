---
name: getbiji-researcher-skill
description: 基于SCAN框架的多源研究技能。结合个人笔记、AI搜索、医学文献进行深度研究分析。当用户需要研究分析、临床证据查找、技术方案调研时使用。
---

# GetBiji Researcher Skill

基于SCAN研究框架，整合多源信息（笔记+搜索+文献）产出结构化研究报告。

## 触发条件

用户意图包含以下关键词时激活：
- 医学类：临床、治疗、药物、副作用、预后、检查、指标、癌症、肿瘤、胰腺、化疗、放疗、免疫
- 技术类：架构、方案、设计、性能、优化、实现、代码、系统、服务
- 通用类：分析、调研、对比、总结、研究

## MCP工具绑定

### 核心必选（每次调用）
- `get-notes-multi-kbs` - 个人知识库检索（第一信源）
- `metaso` - AI智能搜索

### 医学类追加
- `pubmed-data-server` - PubMed文献检索
- `knows-mcp` - 医学指南/论文

### 复杂问题
- `sequential-thinking` - 分步推理

## SCAN 研究流程

### S - Survey/Scout（侦察）

**目标**：搞清楚研究什么 + 需要哪些信息源

**并行调用**：
```
1. get-notes-multi-kbs.searchNotes(keywords, limit=10)
2. metaso.search(topic, mode="deep")
3. pubmed-data-server.search(query, limit=10)  # 医学类
```

**输出**：
- 笔记候选列表（ID + 标题 + 摘要）
- metaso搜索结果
- PubMed文献列表（医学类）

### C - Clarify/Compare（澄清与比较）

**目标**：评估信息质量，初步对比

**权重分配**：
| 来源 | 权重 |
|------|------|
| 医学指南 | 0.95 |
| 个人笔记 | 0.85 |
| 医学论文 | 0.85-0.90 |
| 会议资料 | 0.75 |
| AI搜索 | 0.70 |
| 通用网页 | 0.50-0.65 |
| LLM推断 | 0.30-0.40 |

**冲突检测**：
- 笔记 vs 指南 → 标记"实践差异"
- 指南 vs 最新论文 → 标记"证据更新中"

### A - Analyze/Assess（分析与评估）

**目标**：深度分析，得出带权重结论

**深度调用**（医学类）：
```
1. knows-mcp.searchGuides(topic)
2. pubmed-data-server.getFullText(pmid_list)
```

**输出**：
- 共识观点（多数来源支持）
- 少数派观点
- 不确定性标注

### N - Navigate/Note（导航与记录）

**目标**：形成可执行建议 + 结构化报告

## 输出格式

```markdown
## 研究报告：{topic}

### 1. 背景与问题定义
{问题描述}

### 2. 证据来源概览
| 来源类型 | 数量 | MCP来源 |
|----------|------|---------|
| 个人笔记 | N | get-notes-multi-kbs |
| 医学指南 | N | knows-mcp |
| PubMed文献 | N | pubmed-data-server |
| 综合搜索 | N | metaso |

### 3. 核心发现
{结构化发现}

### 4. 风险与分歧点
- [分歧1] {来源A} vs {来源B}
  - 来源A: {内容} (权重: X)
  - 来源B: {内容} (权重: Y)
  - 建议采信: {决策}

### 5. 推荐方案
{基于证据的推荐}

### 6. 引用索引
- [Note#1] {笔记标题}
- [PubMed#1] PMID:{id} - {标题}
- [Guide#1] {指南名称}
- [Web#1] {来源}

### 7. 置信度评级
整体置信度: 高/中/低
影响因素: {说明}
```

## 使用示例

### 医学研究

**用户问题**：
> RMC6236联合用药的临床数据如何？RVMD官方推荐埃万妥双抗、德曲妥珠联合6236来抑制RTK上调耐药，实际临床联用案例多吗？

**执行流程**：

1. **S阶段**（并行侦察）
   - `get-notes-multi-kbs.searchNotes("RMC6236 联合用药 埃万妥 德曲妥珠")`
   - `metaso.search("RMC6236 combination therapy EGFR MET HER2 clinical trial")`
   - `pubmed-data-server.search("RMC-6236 RAS inhibitor combination resistance")`

2. **C阶段**（比较权重）
   - 标注各来源权重
   - 检测冲突点

3. **A阶段**（深度获取）
   - `knows-mcp.searchPapers("RMC6236 amivantamab trastuzumab deruxtecan")`
   - 获取关键文献全文

4. **N阶段**（输出报告）
   - 按格式输出研究报告

### 技术研究

**用户问题**：
> 基于我的笔记，分析限流方案风险

**执行流程**：

1. **S阶段**
   - `get-notes-multi-kbs.searchNotes("限流 风险")`
   - `metaso.search("2024 微服务限流方案最佳实践")`

2. **C/A阶段**
   - 对比笔记实践 vs 业界最佳实践

3. **N阶段**
   - 输出风险分析报告

## 注意事项

1. **笔记优先**：始终先查个人笔记，这是第一手经验
2. **溯源标注**：每个结论必须标注来源
3. **置信度**：明确标注不确定性
4. **分歧保留**：不同来源冲突时保留两方观点
