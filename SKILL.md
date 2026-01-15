---
name: advanced-get-biji-system
description: 一个多角色协同的智能研究写作系统，基于SCAN框架实现研究、写作、验证一体化工作流
version: 1.0.0
tags: [research, writing, validation, mcp, multi-role, scan-framework]
---

# Advanced GetBiji System

一个多角色协同的智能研究写作系统，基于SCAN框架实现研究、写作、验证一体化工作流。

## 系统概述

Advanced GetBiji System 是一个基于 MCP (Model Context Protocol) 的多角色智能系统，通过 Researcher、Writer、Validator 三个角色的协同工作，实现从研究到写作再到验证的完整工作流。

### 核心特性

- **多角色协同**：Researcher、Writer、Validator 各司其职，协同完成复杂任务
- **SCAN 研究框架**：基于 Survey/Scout、Clarify/Compare、Analyze/Assess、Navigate/Note 框架进行深度研究
- **MCP 集成**：集成多个 MCP 服务，包括笔记检索、AI搜索、医学文献等
- **智能路由**：根据任务类型自动选择合适的 MCP 服务组合
- **质量保证**：通过验证技能确保输出质量和准确性
- **统一输出管理**：所有生成内容统一保存到指定目录

## 系统架构

```
┌─────────────────────────────────────────────────────────────┐
│                    Advanced GetBiji System                 │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────┐ │
│  │   Researcher    │  │     Writer     │  │  Validator  │ │
│  │     Skill       │  │     Skill      │  │    Skill    │ │
│  │  ┌───────────┐  │  │  ┌───────────┐ │  │ ┌─────────┐ │ │
│  │  │ SCAN      │  │  │  │ Writing   │ │  │ │ Quality │ │ │
│  │  │ Framework │  │  │  │ Styles    │ │  │ │ Control │ │ │
│  │  └───────────┘  │  │  └───────────┘ │  │ └─────────┘ │ │
│  └─────────────────┘  └─────────────────┘  └─────────────┘ │
│                              │                              │
│                    ┌─────────┴─────────┐                    │
│                    │    Fusion Layer   │                    │
│                    │  (Result Merging) │                    │
│                    └───────────────────┘                    │
└─────────────────────────────────────────────────────────────┘
```

## 依赖的 MCP 服务

### 必需 MCP 服务
- `get-notes-multi-kbs` - 个人知识库/笔记检索
- `metaso` - AI智能搜索
- `pubmed-data-server` - PubMed医学文献检索
- `sequential-thinking` - 复杂问题分步推理

### 可选 MCP 服务
- `knows-mcp` - 医学指南/论文/会议证据
- `tavily` - 通用网页搜索
- `bingcn` - 必应中文搜索

## 工作流程

### 1. 自动 MCP 检查
```
用户请求 → mcp-check → 服务健康检查 → [条件]启动服务 → 继续执行
```

### 2. 研究阶段
```
1. [并行-侦察] 
   ├─ get-notes-multi-kbs → 检索个人笔记
   ├─ metaso → AI智能搜索
   └─ pubmed-data-server → 医学文献检索 (医学类)

2. [深度分析]
   ├─ knows-mcp → 医学指南验证 (医学类)
   └─ sequential-thinking → 复杂问题分解

3. [证据评估]
   └─ Researcher Skill → 生成研究报告
```

### 3. 写作阶段
```
1. [内容生成]
   └─ Writer Skill → 基于研究结果生成内容

2. [风格适配]
   ├─ 专业风格 → 学术/技术文档
   ├─ 科普风格 → 博客/分享
   └─ 极简风格 → 社交媒体/摘要
```

### 4. 验证阶段
```
1. [质量检查]
   └─ Validator Skill → 验证内容准确性和一致性

2. [可信度评估]
   └─ 生成置信度评级和来源追溯
```

## 输出文件管理

### 默认输出路径
`./generated_reports/` (相对于项目根目录)

### 文件命名规范
- `research_[timestamp]_[topic].md` - 研究报告
- `writing_[timestamp]_[style]_[topic].md` - 写作输出
- `validation_[timestamp]_[topic].md` - 验证报告
- `scan_analysis_[timestamp]_[topic].json` - SCAN分析数据
- `outline_[timestamp]_[style]_[topic].json` - 写作大纲
- `quality_score_[timestamp]_[topic].json` - 质量评分

### 输出示例
```
generated_reports/
├── research_202412011530_RMC6236联合用药.md
├── writing_202412011535_科普_限流方案风险.md
├── validation_202412011540_治疗方案准确性.md
├── scan_analysis_202412011530_RMC6236联合用药.json
└── quality_score_202412011540_治疗方案准确性.json
```

## 使用方法

### ⚡ 快速触发（推荐）

#### 主技能调用 - Advanced GetBiji System
```bash
@advanced-get-biji-system "基于我的笔记，分析[主题]并写[类型]文章"
```

**系统自动执行完整工作流**：
1. ✅ 自动运行 `mcp-check` 检查 MCP 服务状态
2. 🔍 Researcher Skill → 执行 SCAN 研究框架
3. ✍️ Writer Skill → 生成相应风格内容
4. ✔️ Validator Skill → 验证输出质量
5. 💾 自动保存 → `./generated_reports/` 目录

---

### 1. 基础研究任务
```
用户输入: "基于我的笔记，分析[主题]并写[类型]文章"
系统执行:
1. mcp-check → 检查服务状态
2. Researcher Skill → 执行SCAN研究框架
3. Writer Skill → 根据需求生成相应风格内容
4. Validator Skill → 验证输出质量
5. 保存输出 → 保存到 ./generated_reports/ 目录
```

### 2. 专项验证任务
```
用户输入: "验证[内容]的准确性"
系统执行:
1. mcp-check → 检查服务状态
2. Validator Skill → 执行验证流程
3. 保存输出 → 保存验证报告到 ./generated_reports/ 目录
```

### 3. 交互式研究
```
用户输入: "帮我研究[复杂问题]"
系统执行:
1. mcp-check → 检查服务状态
2. Researcher Skill → 执行深度研究
3. [可选] Writer Skill → 生成中间报告
4. [可选] Validator Skill → 验证关键发现
5. 保存输出 → 保存所有输出到 ./generated_reports/ 目录
```

## 配置要求

### 环境变量配置
系统需要以下环境变量（通过 `.env` 文件配置）：

```bash
# 知识库配置
GET_KNOWLEDGE_BASES='[{"id":"demo_group","name":"我的知识库",...}]'

# 搜索服务
METASO_API_KEY=mk-YOUR_METASO_API_KEY

# 医学服务
PUBMED_API_KEY=YOUR_PUBMED_API_KEY
PUBMED_EMAIL=your@email.com
KNOWS_API_KEY=YOUR_KNOWS_API_KEY
KNOWS_API_BASE_URL=https://api.nullht.com

# 其他服务
TAVILY_API_KEY=tvly-YOUR_TAVILY_API_KEY
```

### MCP 服务配置
完整的 MCP 服务配置请参考 `mcp_toolkits.json` 文件。

## 子技能说明

### 1. mcp-check
- **功能**：MCP 服务健康检查与启动
- **位置**：`.agents/skills/mcp-check/`
- **作用**：确保所有必需 MCP 服务在运行其他技能前就绪

### 2. awesome-researcher-skill
- **功能**：基于 SCAN 框架的研究分析
- **位置**：`.agents/skills/awesome-researcher-skill/`
- **作用**：执行深度研究和分析
- **输出**：保存到 `generated_reports/` 目录

### 3. awesome-writer-skill
- **功能**：多风格内容生成
- **位置**：`.agents/skills/awesome-writer-skill/`
- **作用**：根据需求生成不同风格的内容
- **输出**：保存到 `generated_reports/` 目录

### 4. awesome-validator-skill
- **功能**：内容验证与质量控制
- **位置**：`.agents/skills/awesome-validator-skill/`
- **作用**：验证输出的准确性和一致性
- **输出**：保存到 `generated_reports/` 目录

## 安全特性

- **环境变量管理**：所有敏感信息通过环境变量引用
- **权限控制**：各子技能仅访问必需的 MCP 服务
- **配置分离**：全局配置与技能配置分离
- **安全审计**：定期安全检查和配置验证

## 故障处理

### 自动恢复
- MCP 服务未启动时自动启动
- 服务连接失败时自动重试
- 部分服务不可用时启用降级模式

### 降级策略
- `get-notes-multi-kbs` 故障 → 不建议继续（核心知识源丢失）
- `metaso` 故障 → 使用备用搜索服务
- `pubmed-data-server` 故障 → 医学研究降级
- `knows-mcp` 故障 → 验证能力减弱
- `sequential-thinking` 故障 → 复杂推理降级

## 输出格式

系统输出包含以下要素：
- **研究结果**：基于 SCAN 框架的深度分析
- **来源追溯**：所有引用的详细来源信息
- **置信度评级**：对输出内容的可信度评估
- **风险提示**：标识潜在问题和分歧点
- **结构化数据**：便于后续处理的结构化输出
- **统一管理**：所有输出保存到 `generated_reports/` 目录

---

本Skills和作品由小胰宝❤️公益开发，内容由AI生成，**可能存在偏差**，医疗意见请以专业医生和机构意见为准。请关注微信公众号"小胰宝助手"和小X宝社群(https://info.xiao-x-bao.com.cn)，携手推动AI技术开源公益社区，推动肿瘤/罕见病/慢病患者的病情管理提升