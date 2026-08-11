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

## 运行入口硬规则

使用本 Skill 时必须先读取并遵守 `prompts/common_protocol.md` 和 `prompts/dispatcher.md`，不得直接进入写作、检索、审查或交付。

启动顺序固定为：

1. 输出欢迎语。
2. 执行运行能力自测，生成 `capability_snapshot`。
3. 用真实宿主 subagent 调用证据验证多 Agent 支持。
4. 如多 Agent 不支持，必须先让用户选择：`1` 接受降级，或 `2` 取消任务。
5. 用户接受降级后，或多 Agent 验证通过后，进入完整输入对齐。
6. 第一轮输入对齐必须覆盖固定 13 个问题，尤其必须询问目标字数或页数、模板来源、模板族、输出格式、联网检索、人工审核和缺失信息处理方式。

能力判断不得依赖模型自称、角色扮演、单上下文多角色模拟或顺序 prompt 执行。只有 `subagent_probe.passed: true`，且证据中存在独立子代理 `agent_result`、独立 `run_id`、`independent_context: true` 和 `returned_agent_result: true` 时，才允许写 `multi_agent_supported: true`。

任一代理返回 `NEED_USER_INPUT` 时，`questions_for_user` 必须非空，并且每个 `missing_items` 必须映射到可直接回答的问题。不得输出空问题后继续流程。

文献检索必须联网；若运行环境无法联网，不得生成正式参考文献，不得跳过引用核验、全文 AI 味审查、合规审查或交付检查。

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
