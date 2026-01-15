# Advanced-get-biji-Skills

多角色协同研究技能系统，基于SCAN框架实现研究→写作→验证工作流。

## 项目结构

```
advance_get_biji_skills/
├── AGENTS.md                    # 本文件
├── design.md                    # 设计文档（当前版本）
├── design_v*.md                 # 历史版本
├── mcp_toolkits.json           # MCP配置参考
└── .agents/skills/
    ├── getbiji-researcher-skill/   # 研究技能
    │   ├── SKILL.md                # SCAN研究框架
    │   ├── mcp.json                # 5个MCP绑定
    │   └── reference/
    │       └── routing-rules.md    # 路由规则
    ├── getbiji-writer-skill/       # 写作技能
    │   ├── SKILL.md                # 3种风格模板
    │   ├── mcp.json                # 2个MCP绑定
    │   └── reference/
    │       └── style-guide.md      # 风格指南
    └── getbiji-validator-skill/    # 验证技能
        ├── SKILL.md                # 主张验证流程
        └── mcp.json                # 3个MCP绑定
```

## 开发阶段

| 阶段 | 状态 | 交付物 |
|------|------|--------|
| MVP | ✅ 完成 | Researcher Skill + SCAN + 3核心MCP |
| P1 | ✅ 完成 | Writer Skill(3风格) + Validator Skill |
| P2 | 待开发 | 缓存优化 + 更多领域路由 |

## 核心MCP

| MCP | 用途 | 优先级 |
|-----|------|--------|
| get-notes-multi-kbs | 个人笔记检索 | ★★★ 必选 |
| metaso | AI智能搜索 | ★★★ 必选 |
| pubmed-data-server | PubMed文献 | ★★★ 医学必选 |
| knows-mcp | 医学指南/论文 | ★★ 医学增强 |
| sequential-thinking | 分步推理 | ★ 复杂问题 |

## 测试命令

```bash
# 验证skill结构
ls -la .agents/skills/getbiji-researcher-skill/

# 查看MCP配置
cat .agents/skills/getbiji-researcher-skill/mcp.json
```

## 开发记录

- **缓存决策**：MVP阶段不实现缓存，P2阶段按需添加
- **MCP配置**：完整配置见 mcp_toolkits.json
