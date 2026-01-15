# 路由规则参考

## 意图识别关键词

### 医学研究类
```
临床, 治疗, 药物, 副作用, 预后, 检查, 指标, 癌症, 肿瘤, 胰腺, 化疗, 放疗, 免疫,
RAS, KRAS, EGFR, HER2, MET, 靶向, 耐药, 联合用药, 临床试验
```

**必选MCP**: get-notes-multi-kbs, metaso, pubmed-data-server, knows-mcp

### 技术研究类
```
架构, 方案, 设计, 性能, 优化, 实现, 代码, 系统, 服务, 限流, 熔断, 微服务
```

**必选MCP**: get-notes-multi-kbs, metaso

### 通用研究类
```
分析, 调研, 对比, 总结, 研究
```

**必选MCP**: get-notes-multi-kbs, metaso

## 权重配置

```json
{
  "weights": {
    "medical_guideline": 0.95,
    "personal_note": 0.85,
    "pubmed_paper": 0.87,
    "conference_meeting": 0.75,
    "metaso_search": 0.70,
    "web_search": 0.55,
    "llm_inference": 0.35
  }
}
```

## 冲突处理策略

| 冲突类型 | 标记 | 处理方式 |
|----------|------|----------|
| 笔记 vs 指南 | 实践差异 | 保留两方，倾向指南 |
| 指南 vs 最新论文 | 证据更新中 | 标注时效性差异 |
| 多源一致 | 高置信度 | 直接采信 |
| 孤证 | 待验证 | 降低权重，标注不确定 |
