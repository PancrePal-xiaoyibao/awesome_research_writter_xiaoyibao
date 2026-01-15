# MCP 服务故障排查指南

## 环境变量配置

### 1. 设置环境变量

确保 `.env` 文件中包含以下配置：

```bash
# GetBiji Skills 环境变量配置
# 复制此文件为 .env 并填入真实值

# ========== 知识库配置 ==========
GET_KNOWLEDGE_BASES='[{"id":"demo_group","name":"我的知识库","config":{"api_key":"YOUR_API_KEY","topics":[{"id":"topic1","name":"主题1","topic_id":"YOUR_TOPIC_ID"}]}}]'

# ========== Metaso AI搜索 ==========
METASO_API_KEY=mk-YOUR_METASO_API_KEY

# ========== PubMed ==========
PUBMED_API_KEY=YOUR_PUBMED_API_KEY
PUBMED_EMAIL=your@email.com

# ========== Knows医学知识库 ==========
KNOWS_API_KEY=YOUR_KNOWS_API_KEY
KNOWS_API_BASE_URL=https://api.nullht.com

# ========== Tavily搜索 ==========
TAVILY_API_KEY=tvly-YOUR_TAVILY_API_KEY
```

### 2. 替换脚本中的硬编码密钥

#### 错误示例（硬编码密钥）：
```bash
# ❌ 不要这样做 - 包含硬编码密钥
export PUBMED_API_KEY="9c1f5cb4ff62971cf4b383e944913da50808"
```

#### 正确示例（使用环境变量）：
```bash
# ✅ 正确做法 - 从环境变量加载
export $(cat .env | grep -v '^#' | xargs)
# 现在可以使用 $PUBMED_API_KEY 等变量
```

## 常见问题排查

### 1. MCP 服务启动失败

**症状**：
- 服务无法启动
- 连接被拒绝
- API 响应错误

**解决方案**：
1. 检查 `.env` 文件是否存在
2. 验证环境变量是否正确加载
3. 确认 API 密钥是否有效

```bash
# 检查环境变量
echo $METASO_API_KEY
echo $PUBMED_API_KEY
```

### 2. 权限验证失败

**症状**：
- HTTP 401/403 错误
- 认证失败

**解决方案**：
1. 检查 API 密钥格式
2. 验证 API 密钥是否过期
3. 确认 API 密钥权限范围

### 3. 服务连接超时

**症状**：
- 请求超时
- 网络连接失败

**解决方案**：
1. 检查网络连接
2. 验证服务端点 URL
3. 确认防火墙设置

## 安全最佳实践

### 1. 环境变量使用原则

- 所有敏感信息（API 密钥、密码、令牌）必须通过环境变量引用
- 配置文件中不包含任何硬编码的敏感信息
- `.env` 文件必须添加到 `.gitignore`

### 2. 配置文件安全

使用 `${VARIABLE_NAME}` 语法引用环境变量：

```json
{
  "env": {
    "API_KEY": "${MY_API_KEY}",
    "DATABASE_URL": "${DATABASE_URL}"
  }
}
```

### 3. 敏感信息管理

- 不要在代码中硬编码任何敏感信息
- 使用 `.env.example` 提供变量模板
- 定期轮换 API 密钥

## 部署检查清单

- [ ] `.env` 文件已正确配置
- [ ] 所有 API 密钥有效且未过期
- [ ] 配置文件使用环境变量引用
- [ ] `.gitignore` 包含 `.env`
- [ ] 服务启动脚本从环境变量加载配置
- [ ] 故障转移配置已设置

## 故障排查命令

```bash
# 检查环境变量
env | grep -E "(API_KEY|TOKEN|SECRET)"

# 检查 MCP 进程
ps aux | grep -E "(npx.*mcp|get_notebook|pubmed|knows)"

# 查看日志
tail -f /tmp/*.log

# 检查端口占用
netstat -an | grep LISTEN

# 验证 API 连接
curl -H "Authorization: Bearer $METASO_API_KEY" https://metaso.cn/api/mcp/spec
```