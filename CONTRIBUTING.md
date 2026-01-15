# 贡献指南 | Contributing Guide

感谢您对 Advanced GetBiji System 的关注！我们欢迎任何形式的贡献。

## 📋 目录

- [行为准则](#行为准则)
- [如何贡献](#如何贡献)
- [开发流程](#开发流程)
- [代码规范](#代码规范)
- [提交指南](#提交指南)

## 行为准则

### 我们的承诺

为了营造开放和受欢迎的环境，我们作为贡献者和维护者承诺，无论年龄、体型、残疾、种族、性别、身份认同与表达、经验水平、国籍、个人外表、宗教或性认同和性取向，为项目和社区的每个成员提供无骚扰的体验。

### 我们的标准

有助于创造积极环境的行为包括：

- 使用热情和包容的语言
- 对不同的观点和经验表示尊重
- 欣然接受建设性批评
- 关注最对社区有利的事情
- 对其他社区成员表现出同情

不可接受的行为包括：

- 使用性语言或性意象
- 人身攻击、挖苦或贬低评论
- 公开或私下骚扰
- 发布他人的私人信息，如物理或电子地址，未经明确许可
- 其他明显不适当的专业行为

## 如何贡献

### 报告 Bug 🐛

在创建 Bug 报告之前，请检查 issues 列表，因为您可能会发现不需要创建新问题。当您创建 Bug 报告时，请包括尽可能多的详细信息：

- **明确的描述** - 问题的清晰描述
- **重现步骤** - 重现问题的具体步骤
- **预期行为** - 应该发生的具体行为
- **实际行为** - 实际发生的情况
- **截图** - 如果可能，包含屏幕截图
- **环境** - 您的操作系统、Node.js 版本等

### 功能建议 💡

功能建议通过 GitHub issues 进行。创建建议时，请包括：

- **清晰的标题和描述** - 功能是什么以及它解决什么问题
- **可能的实现方式** - 如果您有想法
- **为什么这很有用** - 解释用例和受益者
- **其他背景** - 任何其他上下文或截图

### Pull Requests 🔧

- 填写提供的 PR 模板
- 遵循 JavaScript/TypeScript 风格指南
- 包含适当的测试用例
- 更新相关文档
- 确保 CI/CD 管道通过

## 开发流程

### 1. Fork 仓库

```bash
# 点击 GitHub 上的 "Fork" 按钮
```

### 2. Clone 您的 fork

```bash
git clone https://github.com/YOUR-USERNAME/advance_get_biji_skills.git
cd advance_get_biji_skills
```

### 3. 添加上游远程

```bash
git remote add upstream https://github.com/original-owner/advance_get_biji_skills.git
```

### 4. 创建分支

```bash
# 从 main 分支创建新分支
git checkout -b feature/your-feature-name

# 或修复 bug
git checkout -b fix/bug-description
```

### 5. 进行更改

```bash
# 安装依赖
npm install

# 配置环境变量
cp .env.example .env
# 编辑 .env 文件

# 启动 MCP 服务（如果需要）
cd .agents/skills/mcp-check/reference
./start_mcp_services.sh
```

### 6. 测试您的更改

```bash
# 运行测试
npm test

# 如果适用，测试技能
@advanced-get-biji-system "test query"
@awesome-researcher-skill "research test"
@awesome-writer-skill "writing test"
@awesome-validator-skill "validation test"
```

### 7. 提交更改

```bash
git add .
git commit -m "feat: 添加新功能描述"
```

### 8. Push 到您的 fork

```bash
git push origin feature/your-feature-name
```

### 9. 创建 Pull Request

- 在 GitHub 上导航到原始仓库
- 点击 "New Pull Request"
- 选择您的分支并填写模板

## 代码规范

### 文件结构

```
feature/
├── SKILL.md          # 技能定义
├── reference/        # 参考文件
│   ├── index.js
│   └── utils.js
└── mcp.json          # MCP 配置
```

### 命名规范

- **技能名称**: 使用 kebab-case (例如: `getbiji-researcher-skill`)
- **文件名**: 使用 kebab-case (例如: `start_mcp_services.sh`)
- **变量名**: 使用 camelCase (例如: `outputPath`)
- **常量**: 使用 UPPER_SNAKE_CASE (例如: `DEFAULT_TIMEOUT`)

### 文档规范

- 使用中英文双语（如果适用）
- 在 SKILL.md 中清晰描述功能
- 提供使用示例
- 文档应保持最新

### 提交信息规范

遵循 Conventional Commits 规范：

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Type**:
- `feat`: 新功能
- `fix`: 修复 bug
- `docs`: 文档更新
- `style`: 代码风格（不影响功能）
- `refactor`: 代码重构
- `test`: 添加或更新测试
- `chore`: 构建工具或依赖更新

**示例**:
```
feat(researcher): 添加 SCAN 框架支持

实现了 SCAN 框架的完整研究流程，包括：
- Survey/Scout 侦察阶段
- Clarify/Compare 澄清比较阶段
- Analyze/Assess 深度分析阶段
- Navigate/Note 导航记录阶段

Fixes #123
```

## 提交指南

### Before Submitting

- 确保您的代码遵循项目的风格指南
- 性能：考虑您的更改的性能影响
- 文档：更新所有相关文档
- 测试：添加适当的测试用例

### Review Process

1. 至少一位维护者将审查您的 PR
2. 可能会要求进行更改或改进
3. 一旦批准，您的 PR 将被合并

### After Merge

- 您的贡献将出现在下一个版本中
- 您将被添加到贡献者列表中
- 感谢您的贡献！🎉

## 问题？

有任何问题吗？

- 📧 Email: 通过 GitHub issues 联系维护者
- 💬 讨论: 使用 GitHub Discussions
- 🎯 文档: 检查 README.md 和 SKILL.md

---

**再次感谢您的贡献！** ❤️

本项目由小胰宝❤️公益开发，感谢所有贡献者的支持。
