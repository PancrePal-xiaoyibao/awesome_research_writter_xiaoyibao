# MCP 服务检查与启动技能

## 概述

mcp-check 是一个用于检查和启动 MCP 服务的技能，确保所有必需的 MCP 服务在运行其他技能前处于活动状态。

## 功能特点

- **健康检查**：检查所有必需的 MCP 服务是否正常运行
- **自动启动**：启动未运行的 MCP 服务
- **状态验证**：验证服务的可用性
- **环境管理**：使用环境变量管理敏感信息
- **故障恢复**：提供服务故障的恢复机制

## 配置要求

### 环境变量

确保 `.env` 文件中包含以下环境变量：

```bash
# GetBiji Skills 环境变量配置
GET_KNOWLEDGE_BASES='[{"id":"demo_group","name":"我的知识库","config":{"api_key":"YOUR_API_KEY","topics":[{"id":"topic1","name":"主题1","topic_id":"YOUR_TOPIC_ID"}]}}]'

METASO_API_KEY=mk-YOUR_METASO_API_KEY

PUBMED_API_KEY=YOUR_PUBMED_API_KEY
PUBMED_EMAIL=your@email.com

KNOWS_API_KEY=YOUR_KNOWS_API_KEY
KNOWS_API_BASE_URL=https://api.nullht.com

TAVILY_API_KEY=tvly-YOUR_TAVILY_API_KEY
```

## 使用方法

### 1. 手动运行健康检查

```bash
# 在 Amp 中运行
@mcp-check
```

### 2. 集成到其他技能

在运行其他 MCP 依赖技能前，先运行 mcp-check：

```bash
# 检查 MCP 服务状态
@mcp-check

# 然后运行其他技能
@getbiji-researcher-skill
```

### 3. 自动集成

在其他技能的启动逻辑中集成 mcp-check 检查：

```javascript
// 伪代码示例
async function runWithHealthCheck() {
  const healthStatus = await runSkill('mcp-check');
  
  if (healthStatus.allServicesReady) {
    // 继续执行其他技能
    return await runSkill('getbiji-researcher-skill');
  } else {
    throw new Error(`MCP 服务未就绪: ${healthStatus.failedServices}`);
  }
}
```

## 服务检查清单

| 服务 | 检查项 | 说明 |
|------|--------|------|
| `get-notes-multi-kbs` | 进程运行 + API 响应 | 笔记检索服务 |
| `metaso` | HTTP API 连接 | AI 搜索服务 |
| `pubmed-data-server` | 进程运行 + API 响应 | PubMed 数据服务 |
| `knows-mcp` | 进程运行 + API 响应 | 医学知识库服务 |
| `sequential-thinking` | 进程运行 + API 响应 | 顺序思维服务 |

## 输出格式

### 成功示例

```
✅ MCP 服务检查完成 (耗时: 2.4s)

活跃服务:
- get-notes-multi-kbs: ✅ (响应时间: 0.3s)
- metaso: ✅ (响应时间: 0.8s)  
- pubmed-data-server: ✅ (响应时间: 0.5s)
- knows-mcp: ✅ (响应时间: 0.7s)
- sequential-thinking: ✅ (响应时间: 0.2s)

结论: 所有服务正常，可以安全使用其他技能
```

### 失败示例

```
⚠️ MCP 服务检查完成 (耗时: 5.2s)

部分服务异常:
- get-notes-multi-kbs: ❌ (未启动)
- metaso: ✅ (响应时间: 0.8s)
- pubmed-data-server: ❌ (连接超时)

正在启动未运行的服务...
启动完成，验证状态...

重试后状态:
- get-notes-multi-kbs: ✅ (已启动)
- pubmed-data-server: ✅ (已启动)

结论: 所有服务现在正常，可以安全使用其他技能
```

## 与系统集成

### 前置检查

```
其他技能运行前 → 调用 mcp-check → 验证结果 → 继续执行
```

### 故障处理

- **轻度故障**：自动重试并重新验证
- **严重故障**：提供降级策略或终止执行
- **持久故障**：输出详细错误信息和解决建议

## 安全最佳实践

- 所有敏感信息通过环境变量引用
- 不在配置文件中硬编码任何密钥
- 定期验证 API 密钥的有效性
- 使用最小权限原则配置服务访问

## 故障排查

### 常见问题

1. **环境变量未设置** - 检查 .env 文件
2. **端口占用** - 检查进程冲突
3. **网络问题** - 检查 API 访问

### 排查命令

```bash
# 查看运行进程
ps aux | grep -E "(npx.*mcp|get_notebook|pubmed|knows)"

# 查看日志
tail -f /tmp/*.log

# 重启 MCP 服务
pkill -f "npx.*mcp"; sleep 3; # 然后重新启动
```

---

本Skills和作品由小胰宝❤️公益开发，内容由AI生成，**可能存在偏差**，医疗意见请以专业医生和机构意见为准。请关注微信公众号"小胰宝助手"和小X宝社群(https://info.xiao-x-bao.com.cn)，携手推动AI技术开源公益社区，推动肿瘤/罕见病/慢病患者的病情管理提升