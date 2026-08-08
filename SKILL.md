---
name: xxx-skill
description: Use when generating, auditing, and delivering Chinese research project proposals with multi-agent prompts, fixed templates, citations, AI style review, compliance audit, Markdown JSON DOCX outputs, and degradation handling.
---

# SKILL: xxx-skill

## 元数据

- **skill_id**: `xxx-skill`
- **version**: `0.2.0`
- **scope**: 科研项目申请书多 Agent 生成、审查与交付
- **license**: MIT

## 能力声明

本 Skill 根据用户提供的项目标题、参考资料和可选模板，生成科研项目申请书。系统默认启用人工审核，使用多 Agent 协作完成模板解析、内容分析、大纲设计、章节写作、联网文献检索、正文整合、引用核验、全文 AI 味审查、合规审查和最终 Markdown/JSON/DOCX 交付。

## 输入契约

```yaml
required:
  project_title: string
  reference_materials:
    - string

optional:
  template_ref: string
  template_type:
    enum:
      - basic_research
      - mission_rnd
      - social_science
      - industry_rnd
      - platform_construction
      - compact_proposal
  funding_program: string
  research_scope:
    enum: [web_search, academic_db]
    default: web_search
  output_formats:
    items:
      enum: [markdown, json, docx]
    default: [markdown, json]
  human_review:
    enabled: true
    gates:
      - post_intake
      - degradation_confirm
      - post_template
      - post_outline
      - post_audit
  runtime_mode:
    enum:
      - parallel_multi_agent
      - sequential_multi_role
      - single_agent_compact
    default: parallel_multi_agent
```

文献检索必须联网。若运行环境无法联网或没有学术数据库能力，不得生成正式参考文献。

## 输出契约

```yaml
FinalPackage:
  main_document_md: string
  metadata_json: object
  reference_list_json: object
  main_document_docx: string | null
  assumptions_md: string
  audit_report_json: object
  summary_md: string
  manifest_json: object
  status: READY_FOR_DELIVERY | DELIVERED_WITH_WARNINGS | BLOCKED
```

## Agent 拓扑

| Agent | 职责 |
|---|---|
| Dispatcher | 总控调度、状态维护、回溯和降级决策 |
| Intake Agent | 输入对齐、缺失信息识别、TaskConfig 生成 |
| Template Analyst | 用户模板解析、内置模板匹配、TemplateProfile 生成 |
| Content Analyst | 从用户资料提取研究问题、目标、方法、基础和指标 |
| Outline Architect | 设计章节结构、写作分工、LogicMap、DoNotWriteList |
| Section Writer(s) | 按章节写作正文，保留引用占位符 |
| Literature Search Backfill | 联网检索真实文献，生成引用数据库和参考文献列表 |
| Integrator | 整合章节、统一术语和逻辑衔接 |
| Citation Verifier | 核验引用真实性、正文支撑关系和参考文献格式 |
| Full Document AI Style Auditor | 审查并降低全文 AI 味，不改变事实和引用关系 |
| Compliance Auditor | 检查模板、篇幅、必填项、LogicMap 和交付就绪度 |
| Delivery Agent | 组装 Markdown/JSON/DOCX 交付包 |
| Human Gate | 人工审核、降级确认、强制交付确认和取消任务记录 |

## 人工闸口

默认启用：

- `post_intake`
- `degradation_confirm`
- `post_template`
- `post_outline`
- `post_audit`

用户明确关闭人工审核时，必须记录 `human_review_override`。用户取消任务时，状态为 `CANCELLED_BY_USER`。

## 内置资源

- 6 个模板族，见 `config/templates/`。
- 13 个主 Agent 提示词，见 `prompts/README.md`。
- 公共状态协议，见 `prompts/common_protocol.md`。
- JSON Schema，见 `schemas/`。
