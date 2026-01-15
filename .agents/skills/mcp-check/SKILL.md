---
name: mcp-check
description: MCP服务状态检查与启动技能。检查所有必需的MCP服务是否正常运行，如未启动则自动启动并验证就绪状态。
---

# MCP 服务检查与启动技能

检查并确保所有必需的 MCP 服务处于活动状态，为其他技能提供可靠的服务基础。

## 触发条件

- 在运行其他 MCP 依赖技能前（如 awesome-researcher-skill、awesome-writer-skill、awesome-validator-skill）
- 用户主动请求 MCP 服务诊断
- 系统启动或重启后

## MCP 工具绑定

无直接 MCP 工具依赖，但负责检查以下服务：

- `get-notes-multi-kbs` - 笔记检索服务
- `metaso` - AI 搜索服务（HTTP）
- `pubmed-data-server` - PubMed 数据服务
- `knows-mcp` - 医学知识库服务
- `sequential-thinking` - 顺序思维服务

## 检查清单

| 服务 | 检查项 | 超时 |
|------|--------|------|
| `get-notes-multi-kbs` | 进程运行 + API 响应 | 5s |
| `metaso` | HTTP API 连接 | 3s |
| `pubmed-data-server` | 进程运行 + API 响应 | 5s |
| `knows-mcp` | 进程运行 + API 响应 | 5s |
| `sequential-thinking` | 进程运行 + API 响应 | 3s |

## 执行流程

```
1. [并行] 检查所有 MCP 服务状态
2. [条件] 启动未运行的服务
3. [等待] 服务就绪延时
4. [验证] 服务可用性确认
5. [输出] 检查报告
```

## 输出格式

### ✅ 全部正常
```
MCP 服务健康检查报告
┌─────────────────────────────┐
│  所有 MCP 服务正常运行      │
├─────────────────────────────┤
│  ✓ get-notes-multi-kbs      │
│  ✓ metaso (HTTP)            │
│  ✓ pubmed-data-server       │
│  ✓ knows-mcp                │
│  ✓ sequential-thinking      │
└─────────────────────────────┘

结论: 可以安全启动其他技能
```

### ⚠️ 部分故障
```
MCP 服务健康检查报告
┌─────────────────────────────┐
│  检测到 MCP 服务异常        │
├─────────────────────────────┤
│  ✅ get-notes-multi-kbs     │
│  ❌ pubmed-data-server      │
│  ⚠️  knows-mcp (响应慢)     │
└─────────────────────────────┘

故障服务处理:
- pubmed-data-server: 启动中...
- knows-mcp: 重新连接中...

重试后状态:
- pubmed-data-server: ✅ 已启动
- knows-mcp: ✅ 已恢复
```

### ❌ 全部故障
```
MCP 服务健康检查报告
┌─────────────────────────────┐
│  所有 MCP 服务异常          │
├─────────────────────────────┤
│  ❌ 所有服务均未运行        │
└─────────────────────────────┘

正在启动所有 MCP 服务...
启动完成，验证状态...
```

## 降级策略

- `get-notes-multi-kbs` 故障: 不建议继续（核心知识源丢失）
- `metaso` 故障: 使用备用搜索
- `pubmed-data-server` 故障: 医学研究降级
- `knows-mcp` 故障: 验证能力减弱
- `sequential-thinking` 故障: 复杂推理降级

## 使用示例

### 输入
```
检查 MCP 服务状态
```

### 执行
1. 并行检查所有 MCP 服务进程
2. 验证 API 连接性
3. 生成健康报告
4. 启动未运行的服务

### 输出
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

## 与系统集成

### 前置检查
```
其他技能运行前 → 调用 mcp-check → 验证结果 → 继续执行
```

### 自动重试
- 启动失败时自动重试 3 次
- 间隔 2 秒
- 每次重试后验证状态

## 故障排查

### 常见问题
1. **环境变量未设置** - 检查 .env 文件
2. **端口占用** - 检查进程冲突
3. **网络问题** - 检查 API 访问

### 排查命令
```
# 查看运行进程
ps aux | grep -E "(npx.*mcp|get_notebook|pubmed|knows)"

# 查看日志
tail -f /tmp/*.log

# 重启 MCP 服务
pkill -f "npx.*mcp"; sleep 3; # 然后重新启动
```

---

## 输出与存档

### 保存规则

- **默认输出路径**：`./generated_reports/`
- **文件名格式**：`MCP诊断报告_[日期].md`
- **示例**：`./generated_reports/MCP诊断报告_2026-01-15.md`
- **保存位置**：默认到 `./generated_reports/`，或用户指定路径
- **文件头**：自动添加元数据
  ```markdown
  ---
  title: MCP检查诊断报告
  author: MCP Check Skill
  date: 2026-01-15
  services_checked: 5
  overall_status: 正常/部分故障/严重故障
  ---
  ```

---

## 免责声明与致谢

本Skills和作品由小胰宝❤️公益开发，内容由AI生成，**可能存在偏差**，医疗意见请以专业医生和机构意见为准。

请关注：
- 🔗 **微信公众号**："小胰宝助手"
- 🔗 **小X宝社群**：https://info.xiao-x-bao.com.cn

携手推动AI技术开源公益社区，推动肿瘤/罕见病/慢病患者的病情管理提升