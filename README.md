# Advanced GetBiji Skills - 多角色智能研究写作系统

<div align="center">

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![MCP Compatible](https://img.shields.io/badge/MCP-Compatible-blue.svg)](https://github.com/modelcontextprotocol)
[![AI Research](https://img.shields.io/badge/AI-Research-green.svg)](https://github.com/topics/ai-research)

</div>

一个多角色协同的智能研究写作系统，基于 SCAN 框架实现研究、写作、验证一体化工作流。专为深度研究和高质量内容创作设计，支持个人知识库检索、AI智能搜索、医学文献分析等多种应用场景。

## 🌟 特色功能

### 🔍 多角色协同
- **awesome-researcher-skill**: 基于 SCAN 框架的深度研究分析
- **awesome-writer-skill**: 支持专业、科普、极简三种写作风格
- **awesome-validator-skill**: 内容验证与质量控制
- **mcp-check**: MCP 服务健康检查与启动

### 🧠 SCAN 研究框架
- **Survey/Scout**: 侦察阶段，多源情报收集
- **Clarify/Compare**: 澄清比较，信息去重验证
- **Analyze/Assess**: 深度分析，证据评估
- **Navigate/Note**: 导航记录，结构化输出

### 🔄 MCP 集成
- **get-notes-multi-kbs**: 个人知识库/笔记检索
- **metaso**: AI智能搜索服务
- **pubmed-data-server**: PubMed医学文献检索
- **knows-mcp**: 医学指南/论文/会议证据
- **sequential-thinking**: 复杂问题分步推理

### 📁 统一输出管理
- **默认输出目录**: `generated_reports/`
- **自动文件命名**: 基于时间戳和内容主题
- **多种格式支持**: Markdown 和 JSON 格式
- **自动清理策略**: 保留30天内的文件

## 📋 系统架构

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

## 🚀 快速开始

### 1. 环境准备

首先，确保您的系统满足以下要求：

- Node.js >= 18.0.0
- npm 或 yarn 包管理器
- 访问互联网以使用 MCP 服务

### 2. 配置环境变量

复制环境变量模板并填入您的 API 密钥：

```bash
# 复制模板
cp .env.example .env

# 编辑 .env 文件，填入 API 密钥
nano .env
```

### 3. 启动 MCP 服务

使用提供的启动脚本启动 MCP 服务：

```bash
# 进入 mcp-check 技能的参考目录
cd .agents/skills/mcp-check/reference

# 运行安全启动脚本
./start_mcp_services.sh
```

### 4. 验证服务状态

启动 mcp-check 技能验证 MCP 服务状态：

```bash
# 在 Amp 或支持 MCP 的环境中运行
@mcp-check
```

## 💡 使用示例

### 研究任务

```bash
# 示例：基于个人笔记研究主题
@advanced-get-biji-system "基于我的笔记，分析[主题]的临床数据"
```

### 写作任务

```bash
# 示例：生成科普文章
@awesome-writer-skill "写一篇关于[主题]的科普文章，使用科普风格"
```

### 验证任务

```bash
# 示例：验证内容准确性
@awesome-validator-skill "验证以下内容的准确性：[内容文本]"
```

## 📁 项目结构

```
advance_get_biji_skills/
├── SKILL.md                    # 系统统一接口定义
├── README.md                   # 项目说明文档
├── design.md                   # 系统设计文档
├── mcp_toolkits.json          # MCP 服务配置
├── .env                       # 环境变量配置
├── .env.example              # 环境变量模板
├── .gitignore                # Git 忽略规则
├── generated_reports/        # [NEW] 生成的报告目录
│   ├── research_*.md         # 研究报告
│   ├── writing_*.md          # 写作输出
│   ├── validation_*.md       # 验证报告
│   ├── *.json               # 结构化数据
│   └── ...
│
└── .agents/skills/
    ├── mcp-check/                     # MCP 服务检查
    │   ├── SKILL.md                   # 服务健康检查
    │   ├── mcp.json                   # MCP 配置
    │   └── reference/                 # 参考文件
    │
    ├── awesome-researcher-skill/      # 研究技能
    │   ├── SKILL.md                   # SCAN 研究框架
    │   ├── mcp.json                   # MCP 绑定
    │   └── reference/                 # 参考文件
    │
    ├── awesome-writer-skill/          # 写作技能
    │   ├── SKILL.md                   # 写作风格模板
    │   ├── mcp.json                   # MCP 绑定
    │   └── reference/                 # 参考文件
    │
    └── awesome-validator-skill/       # 验证技能
        ├── SKILL.md                   # 验证流程
        └── mcp.json                   # MCP 绑定
```

## ⚙️ 配置说明

### 环境变量

| 变量名 | 说明 | 必需 |
|--------|------|------|
| `GET_KNOWLEDGE_BASES` | 知识库配置 | ✅ |
| `METASO_API_KEY` | Metaso AI搜索密钥 | ✅ |
| `PUBMED_API_KEY` | PubMed API密钥 | 医学类必需 |
| `PUBMED_EMAIL` | PubMed联系邮箱 | 医学类必需 |
| `KNOWS_API_KEY` | Knows医学知识库密钥 | 医学类必需 |
| `KNOWS_API_BASE_URL` | Knows API基础URL | 医学类必需 |
| `TAVILY_API_KEY` | Tavily搜索密钥 | 可选 |

### MCP 服务配置

MCP 服务配置文件 `mcp_toolkits.json` 定义了所有可用的 MCP 服务器实例。各子技能通过各自的 `mcp.json` 文件定义对 MCP 服务的访问权限。

## 📁 输出文件管理

### 默认输出路径

所有技能的输出文件都保存在 `generated_reports/` 目录中：

```
generated_reports/
├── research_YYYYMMDDHHMM_[topic].md        # 研究报告
├── writing_YYYYMMDDHHMM_[style]_[topic].md # 写作输出
├── validation_YYYYMMDDHHMM_[topic].md      # 验证报告
├── scan_analysis_YYYYMMDDHHMM_[topic].json # SCAN分析数据
├── outline_YYYYMMDDHHMM_[style]_[topic].json # 写作大纲
├── references_YYYYMMDDHHMM_[style]_[topic].json # 引用数据
├── evidence_trail_YYYYMMDDHHMM_[topic].json # 证据链数据
├── quality_score_YYYYMMDDHHMM_[topic].json # 质量评分
└── risk_assessment_YYYYMMDDHHMM_[topic].json # 风险评估
```

### 文件命名规范

- **时间戳格式**: `YYYYMMDDHHMM` (例如：202412011530)
- **内容主题**: 使用下划线分隔的关键词描述内容主题
- **风格标识**: 写作技能包含风格标识（专业/科普/极简）

### 自动清理策略

- **保留期限**: 默认保留30天内的文件
- **定期清理**: 系统可配置定期清理过期文件
- **大文件处理**: 超过10MB的文件进行压缩存储

## 🔐 安全最佳实践

### 环境变量管理
- 所有敏感信息存储在 `.env` 文件中
- 配置文件使用 `${VARIABLE_NAME}` 语法引用环境变量
- `.env` 文件已添加到 `.gitignore`，不会提交到代码仓库

### 权限控制
- 各技能仅访问必需的 MCP 服务
- 全局配置与技能配置分离
- 实现最小权限原则

### 安全审计
- 定期审查配置文件
- 验证环境变量是否正确加载
- 测试 MCP 服务连接性

## 🛠️ 开发计划

| 阶段 | 目标 | 状态 |
|------|------|------|
| **MVP** | 核心研究流程 | ✅ 完成 |
| **P1** | 完整写作验证 | ✅ 完成 |
| **P2** | 系统可靠性 | ✅ 完成 |
| **P3** | 安全加固 | ✅ 完成 |
| **P4** | 技能集成 | ✅ 完成 |
| **P5** | 输出管理 | ✅ 完成 |
| **P6** | 性能优化 | 🔄 进行中 |

## 🤝 贡献指南

我们欢迎任何形式的贡献！请参阅 `CONTRIBUTING.md` 了解更多信息。

### 开发环境设置

```bash
# 克隆仓库
git clone <repository-url>
cd advance_get_biji_skills

# 安装依赖（如果有）
npm install

# 配置环境变量
cp .env.example .env
# 编辑 .env 文件

# 启动 MCP 服务
cd .agents/skills/mcp-check/reference
./start_mcp_services.sh
```

## 📄 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件。

## 🙏 致谢

特别感谢所有为本项目做出贡献的开发者和支持者。

---

<div align="center">

**本项目由小胰宝❤️公益开发**  
内容由AI生成，**可能存在偏差**，医疗意见请以专业医生和机构意见为准。

关注微信公众号"小胰宝助手"和小X宝社群(https://info.xiao-x-bao.com.cn)，携手推动AI技术开源公益社区，推动肿瘤/罕见病/慢病患者的病情管理提升。

</div>