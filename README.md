# xxx-skill

科研项目申请书多 Agent 生成 skill。  
Multi-agent skill for Chinese research proposal drafting, review, and delivery.

它面向低门槛输入场景：用户提供项目标题或研究主题、参考资料，可选提供模板；系统完成输入对齐、模板解析、内容分析、大纲设计、分章节写作、联网文献检索与回填、整合、引用核验、全文 AI 味审查、合规审查和最终交付。  
It is built for low-friction inputs: users provide a project title or research topic, reference materials, and optionally a template; the system handles intake alignment, template analysis, content analysis, outline design, section writing, online literature search and backfill, integration, citation verification, full-document AI-style review, compliance audit, and final delivery.

## 核心原则 / Core Principles

- 默认启用人工审核。  
  Human review is enabled by default.
- 文献检索必须联网；没有联网或学术数据库能力时，不得生成正式参考文献。  
  Literature search must use network access; without online or academic database access, formal references must not be generated.
- 正式支持 Markdown、JSON、DOCX。  
  Markdown, JSON, and DOCX are the supported delivery formats.
- Markdown 是最终交付和格式转换的唯一正文源。  
  Markdown is the single source of truth for final delivery and format conversion.
- 不支持多 Agent 架构时，必须先让用户选择接受降级或取消任务。  
  If the runtime does not support multi-agent execution, the user must first choose degradation or task cancellation.
- 任一 Agent 不得编造团队成果、指标、预算、合作单位、项目经历或正式引用。  
  No agent may invent team achievements, metrics, budgets, partners, project history, or formal citations.

## 快速开始 / Quick Start

最小输入：  
Minimal input:

```yaml
project_title: "项目标题或研究主题"
reference_materials:
  - "./materials/project_outline.md"
template_ref: null
template_type: null
research_scope: web_search
output_formats:
  - markdown
  - json
  - docx
human_review:
  enabled: true
```

用户提供模板时优先使用用户模板；用户模板缺少章节、字段或格式规则时，才从匹配度最高的内置模板族补足，并记录来源和假设。  
When a user template is provided, it has priority; if it is missing sections, fields, or formatting rules, the best-matching built-in template family may be used as a fallback, and the source plus assumptions must be recorded.

## 内置模板族 / Built-in Template Families

主流程只使用以下 6 个模板族：  
The main workflow uses only these 6 template families:

| 模板族 / Family | 适用场景 / Use case |
|---|---|
| `basic_research` | 基础研究、自然科学基金、理论方法类申请 / Basic research, NSFC-style, theory-oriented proposals |
| `mission_rnd` | 任务牵引、重点研发、技术攻关类申请 / Mission-driven R&D, key R&D, technical breakthrough proposals |
| `social_science` | 哲学社会科学、政策研究、管理研究类申请 / Social science, policy research, management research proposals |
| `industry_rnd` | 产业研发、企业技术、成果转化类申请 / Industry R&D, enterprise technology, commercialization proposals |
| `platform_construction` | 平台、基地、实验室、条件建设类申请 / Platform, base, lab, and infrastructure proposals |
| `compact_proposal` | 通用简版、预申报、短版项目建议书 / Compact drafts, pre-submissions, short proposal forms |

模板文件位于 `config/templates/`。  
Template files are located in `config/templates/`.

## 多 Agent 架构 / Multi-Agent Architecture

主流程：  
Main workflow:

```text
Dispatcher
  -> Intake Agent
  -> Template Analyst
  -> Content Analyst
  -> Outline Architect
  -> Section Writer(s)
  -> Literature Search Backfill
  -> Integrator
  -> Citation Verifier
  -> Full Document AI Style Auditor
  -> Compliance Auditor
  -> Delivery Agent
  -> Human Gate
```

末端链路固定为：  
The final chain is fixed as:

```text
Integrator
  -> Citation Verifier
  -> Full Document AI Style Auditor
  -> Compliance Auditor
  -> Delivery Agent
```

旧版五角色提示词已删除，不参与主流程。  
The old five-role prompts have been removed and do not participate in the main workflow.

## 人工审核 / Human Review

默认闸口：  
Default gates:

- `post_intake`：确认输入、缺失信息、假设和输出格式。  
  Confirms inputs, missing information, assumptions, and output formats.
- `degradation_confirm`：当前 SDK 不支持多 Agent 时，确认接受降级或取消任务。  
  Confirms degradation or cancellation when the SDK cannot support multi-agent execution.
- `post_template`：确认模板选择、模板来源和补足规则。  
  Confirms template choice, template source, and fallback rules.
- `post_outline`：确认章节结构、写作分工、LogicMap 和 DoNotWriteList。  
  Confirms section structure, writing assignment, LogicMap, and DoNotWriteList.
- `post_audit`：确认最终审查结果、未解决问题和是否强制交付。  
  Confirms final audit results, unresolved issues, and whether forced delivery is requested.

用户取消任务时，任务状态为 `CANCELLED_BY_USER`。审查未完全通过但用户明确要求强制交付时，最终状态只能是 `DELIVERED_WITH_WARNINGS`。  
If the user cancels the task, the final status is `CANCELLED_BY_USER`. If the review is not fully passed but the user explicitly requests forced delivery, the final status must be `DELIVERED_WITH_WARNINGS`.

## 运行模式 / Runtime Modes

| 模式 / Mode | 说明 / Description |
|---|---|
| `parallel_multi_agent` | 优先模式，多角色并行协作 / Preferred mode, multi-role parallel collaboration |
| `sequential_multi_role` | SDK 不支持并行时，保留角色边界并顺序执行 / Sequential execution while preserving role boundaries |
| `single_agent_compact` | SDK 不支持多 Agent 时，由宿主 Agent 按同一协议顺序模拟各角色 / Single-agent fallback that simulates the same protocol in sequence |

降级只改变执行方式，不改变状态协议、产物契约、人工审核闸口和交付规则。  
Degradation changes only the execution mode, not the state protocol, artifact contract, human review gates, or delivery rules.

## 交付包 / Delivery Package

`FinalPackage` 至少包含：  
`FinalPackage` must include at least:

```text
main_document.md
metadata.json
reference_list.json
assumptions.md
audit_report.json
summary.md
manifest.json
main_document.docx   # 用户要求且转换成功时
```

交付状态：  
Delivery statuses:

- `READY_FOR_DELIVERY`：审查通过，基础文件和用户要求的必需格式均生成并校验成功。  
  Review passed, required files and requested formats were generated and validated successfully.
- `DELIVERED_WITH_WARNINGS`：Markdown 和 JSON 成功生成，但存在用户强制交付、DOCX 失败或其他非阻塞警告。  
  Markdown and JSON were generated successfully, but forced delivery, DOCX failure, or other non-blocking warnings exist.
- `BLOCKED`：缺少形成可交付正文的必要材料，或 Markdown/JSON 基础产物无法生成。  
  Necessary materials for a deliverable draft are missing, or the Markdown/JSON base artifacts cannot be generated.

DOCX 生成后必须校验标题层级、表格、图片、引用编号、中文字体、页数估算和正文一致性。校验失败时自动降级为 Markdown + JSON，并记录警告。  
After DOCX generation, heading levels, tables, images, citation numbers, Chinese fonts, page estimate, and content consistency must be validated. If validation fails, the system must degrade to Markdown + JSON and record a warning.

## 仓库结构 / Repository Structure

```text
xxx_skill/
├── README.md
├── SKILL.md
├── config/
│   ├── skill_config.yaml
│   └── templates/
│       ├── basic_research.md
│       ├── mission_rnd.md
│       ├── social_science.md
│       ├── industry_rnd.md
│       ├── platform_construction.md
│       └── compact_proposal.md
├── prompts/
│   ├── README.md
│   ├── common_protocol.md
│   ├── agent_prompt_template.md
│   ├── dispatcher.md
│   ├── intake_agent.md
│   ├── template_analyst.md
│   ├── content_analyst.md
│   ├── outline_architect.md
│   ├── section_writer*.md
│   ├── literature_search_backfill.md
│   ├── integrator.md
│   ├── citation_verifier.md
│   ├── full_document_ai_style_auditor.md
│   ├── compliance_auditor.md
│   ├── delivery_agent.md
│   └── human_gate.md
└── schemas/
    ├── agent_result.schema.json
    ├── artifact.schema.json
    ├── task_config.schema.json
    ├── template_profile.schema.json
    ├── citation_verification_report.schema.json
    ├── full_document_ai_style_audit.schema.json
    ├── compliance_audit.schema.json
    └── final_package.schema.json
```

## 关键文档 / Key Documents

- [提示词索引 / Prompt Index](prompts/README.md)
- [公共协议 / Common Protocol](prompts/common_protocol.md)
- [Agent 提示词模板 / Agent Prompt Template](prompts/agent_prompt_template.md)
- [全局配置 / Global Config](config/skill_config.yaml)

## License

MIT
