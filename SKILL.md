# SKILL: xxx-skill

## 元数据

- **skill_id**: `xxx-skill`
- **version**: `0.1.0`
- **scope**: 科研课题申请书智能生成（重点研发/国自然/省基金/省级/企业研发）
- **author**: user-defined
- **license**: MIT

## 能力声明

本 Skill 通过多 Agent 流水线，将课题内容大纲与格式模板转化为高质量学术申报书文本。支持全自动模式与人工闸口模式。

## 输入契约

```yaml
required:
  content_ref: string          # 课题大纲文件路径或文本内容

optional:
  format_ref: string           # 格式模板文件路径或文本内容
  doc_type: enum               # 内置模板选择，当 format_ref 缺失时生效
    values: [key_rnd_project, nsfc_general, nsfc_youth, provincial_key, enterprise_rnd]
  mode: enum                   # 运行模式
    values: [heavy, light]
    default: heavy
  research_scope: enum          # 检索范围
    values: [internal_only, web_search, academic_db]
    default: internal_only
  human_review_gates: array    # 人工审核闸口
    items: {enum: [post_draft, post_audit]}
    default: []
  max_iterations: integer       # 最大迭代次数
    default: 3
  strictness_level: enum       # 核查严格度
    values: [strict, normal, loose]
    default: strict
  citation_pool_size: integer   # 引用池上限
    default: 50
  output_formats: array         # 输出格式
    items: {enum: [markdown, json, docx]}
    default: [markdown, json]
```

## 输出契约

```yaml
FinalPackage:
  main_document_md: string      # Markdown 正文
  metadata_json: object        # 结构化元数据（含引用统计、核查分数、迭代次数）
  main_document_docx: string   # Word 文件（base64 或路径，可选）
  audit_summary:
    final_score: integer        # 百分制
    iterations: integer
    unresolved_issues: array
```

## Agent 拓扑

| Agent | 角色 | 实例数 | 输入 | 输出 |
|---|---|---|---|---|
| Dispatcher | 调度中枢 | 1 | User Config + 各Agent输出 | 状态表 + 路由决策 |
| Agent-A | 解析层 | 1 | content_ref, format_ref | ParsedTemplate, ContentOutline, WordBudget |
| Agent-B | 调研层+回填层 | 1 | ContentOutline, Draft | CitationDatabase, BackfilledDocument |
| Agent-C | 生成层 | 3（并行） | ParsedTemplate, CitationDatabase | Draft（含[Ref]占位符） |
| Agent-D | 质控层 | 1 | BackfilledDocument, CitationDatabase, ParsedTemplate | AuditReport |
| Agent-E | 交付层 | 1 | BackfilledDocument, AuditReport | FinalPackage |
| HumanGate | 人机闸口 | 0~2 | Dispatcher暂停信号 | CONTINUE / REWRITE / MODIFY |

## 状态机

```
S1 PARSING → S2 RESEARCH → S3 WRITING → S4 BACKFILL → S5 AUDIT → S6 DELIVERY → S7 DONE
```

闸口插入点：
- GATE_POST_DRAFT: S3 → S4
- GATE_POST_AUDIT: S5 → S6

## 依赖工具（可选）

- `web_search`: Agent-B 外搜增强模式必需
- `web_open_url`: Agent-B 文献验证推荐
- `get_data_source`: Agent-B 学术数据库模式可选
- `ipython`: Agent-E Word 组装可选（需文档生成库）

## 内置资源

- 5 套默认模板（见 `config/templates/`）
- 3 套 JSON Schema（见 `schemas/`）
- 7 份 Agent System Prompt（见 `prompts/`）
