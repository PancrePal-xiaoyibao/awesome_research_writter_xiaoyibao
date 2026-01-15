# 变更日志 (Changelog)

所有值得注意的变更都将在此文件中记录。

## [v1.0.0] - 2026-01-15

### 发布说明

这是 Advanced GetBiji Skills 项目的首个正式版本，提供了一个完整的多角色智能研究写作系统。

### 🌟 主要特性

#### 多角色协同系统
- **Researcher Skill**: 基于 SCAN 框架的深度研究分析
- **Writer Skill**: 支持专业、科普、极简三种写作风格
- **Validator Skill**: 内容验证与质量控制
- **mcp-check Skill**: MCP 服务健康检查与启动

#### SCAN 研究框架
- **Survey/Scout**: 侦察阶段，多源情报收集
- **Clarify/Compare**: 澄清比较，信息去重验证
- **Analyze/Assess**: 深度分析，证据评估
- **Navigate/Note**: 导航记录，结构化输出

#### MCP 服务集成
- **get-notes-multi-kbs**: 个人知识库/笔记检索
- **metaso**: AI智能搜索服务
- **pubmed-data-server**: PubMed医学文献检索
- **knows-mcp**: 医学指南/论文/会议证据
- **sequential-thinking**: 复杂问题分步推理

#### 统一输出管理
- **默认输出目录**: `./generated_reports/`
- **自动文件命名**: 基于时间戳和内容主题
- **多种格式支持**: Markdown 和 JSON 格式

### 🔧 功能改进

#### 技能名称规范化
- 将技能名称从 `getbiji-*` 改为 `awesome-*`
- 更新所有相关引用和文档

#### 输出路径优化
- 将输出路径从绝对路径改为相对路径 `./generated_reports/`
- 提高项目可移植性

#### 文档完善
- 更新 README.md 包含完整的项目说明
- 更新 SKILL.md 包含系统统一接口定义
- 更新设计文档包含最新架构

### 📁 项目结构

```
advance_get_biji_skills/
├── SKILL.md                    # 系统统一接口定义
├── README.md                   # 项目说明文档
├── design.md                   # 系统设计文档
├── mcp_toolkits.json          # MCP 服务配置
├── .env                       # 环境变量配置
├── .env.example              # 环境变量模板
├── .gitignore                # Git 忽略规则
├── generated_reports/        # 生成的报告目录
├── CHANGELOG.md             # 变更日志
├── LICENSE                  # 许可证
├── CONTRIBUTING.md          # 贡献指南
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

### 🚀 快速开始

#### 1. 环境准备
- Node.js >= 18.0.0
- npm 或 yarn 包管理器

#### 2. 配置环境变量
```bash
# 复制模板
cp .env.example .env

# 编辑 .env 文件，填入 API 密钥
nano .env
```

#### 3. 启动 MCP 服务
```bash
# 进入 mcp-check 技能的参考目录
cd .agents/skills/mcp-check/reference

# 运行安全启动脚本
./start_mcp_services.sh
```

#### 4. 使用示例
```bash
# 研究任务
@advanced-get-biji-system "基于我的笔记，分析[主题]并写[类型]文章"

# 写作任务
@awesome-writer-skill "写一篇关于[主题]的科普文章，使用科普风格"

# 验证任务
@awesome-validator-skill "验证以下内容的准确性：[内容文本]"
```

### 🔐 安全最佳实践
- 所有敏感信息通过环境变量管理
- 配置文件使用 `${VARIABLE_NAME}` 语法引用环境变量
- `.env` 文件已添加到 `.gitignore`

---

**版本**: v1.0.0  
**发布日期**: 2026-01-15  
**项目**: Advanced GetBiji Skills - 多角色智能研究写作系统