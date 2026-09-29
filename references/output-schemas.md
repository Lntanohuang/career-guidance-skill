# 就业指导输出结构

当前报告以 HTML 输出，以下表格仅说明内容字段，实际须转成 HTML。岗位数据遵循 [岗位数据展示规则](job-data-presentation.md)：比例优先、统一自有岗位库来源、小样本保留代表性限制。内部原始计数保留用于追溯。

## 岗位匹配表

| 岗位要求 | 类型（必须/优先/可培养） | 对应经历或材料 | 状态（已具备/部分具备/待补强/信息不足） | 下一步验证 |
| --- | --- | --- | --- | --- |

## 岗位需求分析

具体城市和岗位必须来自岗位库查询，用户正文至少说明筛选口径、样本范围、学历/经验/薪资结构和对投递决策的影响；查询不足时不写无来源比例。

## 简历修改建议

至少包含一处“原文 → 建议改写”，并列出改写依赖的事实和仍需补齐的证据。项目指标、技术栈和职责只能来自用户材料。

## 行动计划

| 优先级 | 动作 | 产出物 | 完成标准 | 截止时间 | 预计投入 |
| --- | --- | --- | --- | --- | --- |

## Offer 比较

| 维度 | A 方案 | B 方案 | 已知来源/时间 | 待核实问题 |
| --- | --- | --- | --- | --- |

至少比较：岗位实际职责、直属团队、成长与培训、现金与浮动薪酬、福利、工作地点/通勤、工作时间、合同与试用期、离职约束，以及与个人阶段的匹配度。未知项保持未知，不以默认值填充。

## 历史侧车协议（仅供旧报告维护）

以下内容属于旧版 report-meta 协议，当前 HTML 报告不生成这些 JSON 字段，也不按旧协议禁止 HTML/SVG。不要将这里的示例样本数或数据复制进新报告。

### 开发侧依据字段

该字段随报告侧车输出，供开发人员检查判断质量，不展示给用户：

```json
{
  "evidence": [
    {
      "id": "E1",
      "requirement": "岗位要求",
      "support": "用户材料或岗位原文",
      "status": "matched",
      "sourceIds": ["DS1"]
    }
  ]
}
```

`status` 使用 `matched`、`partial`、`gap` 或 `unknown`。

正式报告侧车可选记录岗位分析与简历审阅摘要：

```json
{
  "marketAnalysis": {
    "status": "complete",
    "queryId": "Q_GZ_JAVA_INTERNSHIP_20260924",
    "filters": { "city": "广州市", "keywords": ["Java", "后端"], "internship": true },
    "sampleSize": 0,
    "snapshotDate": "2026-09-24",
    "metricIds": ["M1"],
    "chartIds": ["CH1"],
    "gaps": []
  },
  "resumeReview": {
    "sourceIds": ["DS_RESUME_1"],
    "strengths": ["…"],
    "missingEvidence": ["…"],
    "rewriteItems": ["…"]
  }
}
```

岗位库查询失败、样本不足或用户材料缺失时，使用开发侧字段记录原因，不在正文增加“缺口与风险”章节：

```json
{
  "gaps": [
    {
      "id": "G1",
      "type": "data",
      "title": "广州 Java 后端实习学历结构",
      "description": "需要查询岗位库后计算本科及以上占比",
      "impact": "影响投递范围建议",
      "status": "pending_query",
      "verification": "按城市、岗位名和在招条件执行串行聚合"
    }
  ],
  "risks": [
    {
      "id": "R1",
      "title": "学历门槛判断待查询确认",
      "severity": "medium",
      "basis": "当前没有有效岗位库结果",
      "mitigation": "查询完成后按真实比例调整投递策略"
    }
  ]
}
```

具体城市、岗位、实习/应届或学历门槛问题必须调用 MySQL 只读岗位库；查询结果必须作为 `sources`、`metrics` 和 `charts` 的来源。查询失败时不生成比例图表。

## 图表规格（开发侧）

需要图表时，报告侧车使用 `report-meta/2` 的可选 `charts` 字段。Agent 只生成数据描述，前端负责渲染：

```json
{
  "charts": [
    {
      "id": "salary-distribution",
      "type": "bar",
      "title": "高职核心岗位薪资分布",
      "data": [
        { "label": "3–6K", "value": 32.6 },
        { "label": "6–10K", "value": 40.3 }
      ],
      "unit": "%",
      "sourceIds": ["DS_JOB_20260920"],
      "snapshotDate": "2026-09-20",
      "caveat": "仅代表岗位库内部结构，不代表全国岗位总量",
      "altText": "6–10K 占比最高，为 40.3%"
    }
  ]
}
```

允许的 `type` 为 `bar`、`stackedBar`、`histogram`、`line`、`map`。不要在 `charts` 中放 HTML、SVG、JavaScript 或图片数据；缺少来源、单位或统计口径时不生成图表。
