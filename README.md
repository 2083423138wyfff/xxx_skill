# xxx-skill

科研项目申请书多 Agent 生成 skill。它面向低门槛输入场景：用户提供项目标题或研究主题、参考资料，可选提供模板；系统完成输入对齐、模板解析、内容分析、大纲设计、分章节写作、联网文献检索与回填、整合、引用核验、全文 AI 味审查、合规审查和最终交付。

## 核心原则

- 默认启用人工审核。
- 文献检索必须联网；没有联网或学术数据库能力时，不得生成正式参考文献。
- 正式支持 Markdown、JSON、DOCX。
- Markdown 是最终交付和格式转换的唯一正文源。
- 不支持多 Agent 架构时，必须先让用户选择接受降级或取消任务。
- 任一 Agent 不得编造团队成果、指标、预算、合作单位、项目经历或正式引用。

## 快速开始

最小输入：

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

## 内置模板族

主流程只使用以下 6 个模板族：

| 模板族 | 适用场景 |
|---|---|
| `basic_research` | 基础研究、自然科学基金、理论方法类申请 |
| `mission_rnd` | 任务牵引、重点研发、技术攻关类申请 |
| `social_science` | 哲学社会科学、政策研究、管理研究类申请 |
| `industry_rnd` | 产业研发、企业技术、成果转化类申请 |
| `platform_construction` | 平台、基地、实验室、条件建设类申请 |
| `compact_proposal` | 通用简版、预申报、短版项目建议书 |

模板文件位于 `config/templates/`。

## 多 Agent 架构

主流程：

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

```text
Integrator
  -> Citation Verifier
  -> Full Document AI Style Auditor
  -> Compliance Auditor
  -> Delivery Agent
```

## 人工审核

默认闸口：

- `post_intake`：确认输入、缺失信息、假设和输出格式。
- `degradation_confirm`：当前 SDK 不支持多 Agent 时，确认接受降级或取消任务。
- `post_template`：确认模板选择、模板来源和补足规则。
- `post_outline`：确认章节结构、写作分工、LogicMap 和 DoNotWriteList。
- `post_audit`：确认最终审查结果、未解决问题和是否强制交付。

用户取消任务时，任务状态为 `CANCELLED_BY_USER`。审查未完全通过但用户明确要求强制交付时，最终状态只能是 `DELIVERED_WITH_WARNINGS`。

## 运行模式

| 模式 | 说明 |
|---|---|
| `parallel_multi_agent` | 优先模式，多角色并行协作 |
| `sequential_multi_role` | SDK 不支持并行时，保留角色边界并顺序执行 |
| `single_agent_compact` | SDK 不支持多 Agent 时，由宿主 Agent 按同一协议顺序模拟各角色 |

降级只改变执行方式，不改变状态协议、产物契约、人工审核闸口和交付规则。

## 交付包

`FinalPackage` 至少包含：

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

- `READY_FOR_DELIVERY`：审查通过，基础文件和用户要求的必需格式均生成并校验成功。
- `DELIVERED_WITH_WARNINGS`：Markdown 和 JSON 成功生成，但存在用户强制交付、DOCX 失败或其他非阻塞警告。
- `BLOCKED`：缺少形成可交付正文的必要材料，或 Markdown/JSON 基础产物无法生成。

DOCX 生成后必须校验标题层级、表格、图片、引用编号、中文字体、页数估算和正文一致性。校验失败时自动降级为 Markdown + JSON，并记录警告。

## 仓库结构

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

## 关键文档

- [提示词索引](prompts/README.md)
- [公共协议](prompts/common_protocol.md)
- [Agent 提示词模板](prompts/agent_prompt_template.md)
- [全局配置](config/skill_config.yaml)

## License

MIT
