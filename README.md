# xxx-skill

> **脏活累活（科研课题申请书）多Agent智能生成系统**  
> 基于内容大纲与格式模板，通过多Agent流水线自动生成高质量科研申请书（重点研发、国自然、省基金等）。

---

## 核心特性

- **模板驱动骨架**：内置 5 套常见科研申请书模板，也支持自定义格式参考文件
- **内容驱动血肉**：解析课题大纲，自动提取研究方向、技术关键词、依赖关系
- **Agent流水线驱动质量**：7阶段状态机（解析→检索→撰写→回填→核查→迭代→交付）
- **人机闸口驱动可控性**：支持 `post_draft`（草稿确认）和 `post_audit`（核查确认）人工审核节点
- **引用分池管理**：国外文献[1-N]、国内文献[N+1-M]、技术原理[M+1-K]，避免混淆
- **防幻觉协议**：所有引用必须带 `source`，无法验证的标记 `verification_needed`

---

## 快速开始

### 1. 全自动模式（最快）

```yaml
content_ref: "./my_project_outline.md"   # 你的课题大纲（必须）
format_ref: "./guide_template.md"        # 格式模板（可选，无则加载内置模板）
config:
  mode: heavy
  research_scope: internal_only
  human_review_gates: []
  output_formats: [markdown, json]
```

### 2. 高质量模式（外搜增强 + 闸口审核）

```yaml
content_ref: "./my_project_outline.md"
format_ref: "./guide_template.md"
config:
  mode: heavy
  research_scope: web_search              # 需要为 Agent-B 绑定 web_search 工具
  human_review_gates: [post_draft, post_audit]
  output_formats: [markdown, json, docx]
```

> **前置要求**：若使用 `web_search` 或 `academic_db`，请确保 Agent-B 实例已开启 Function Calling 并绑定搜索工具。

---

## 仓库结构

```
xxx-skill/
├── README.md                    # 本文件
├── SKILL.md                     # AI平台读取的Skill元数据定义
├── config/
│   ├── skill_config.yaml         # 全局配置Schema
│   └── templates/                # 内置模板库
│       ├── key_rnd_project.md    # 国家重点研发计划
│       ├── nsfc_general.md       # 国自然面上项目
│       ├── nsfc_youth.md        # 国自然青年基金
│       ├── provincial_key.md    # 省级重点研发
│       └── enterprise_rnd.md    # 企业技术研发
├── prompts/                      # Agent System Prompts
│   ├── dispatcher.md            # 调度中枢
│   ├── agent_a_parser.md        # 解析层
│   ├── agent_b_researcher.md    # 调研层 + 回填层
│   ├── agent_c_writer.md        # 生成层（3实例共用）
│   ├── agent_d_auditor.md       # 质控层
│   ├── agent_e_assembler.md     # 交付层
│   └── human_gate.md           # 人机闸口
└── schemas/                     # JSON Schema 数据契约
    ├── parsed_template.schema.json
    ├── citation_db.schema.json
    └── audit_report.schema.json
```

---

## 内置模板对照表

| 模板ID | 适用场景 | 特殊字段 |
|---|---|---|
| `key_rnd_project` | 国家重点研发计划 | 城市比较、本地单位线索、产业链关键瓶颈 |
| `nsfc_general` | 国自然面上项目 | 无产业化字段，重基础研究 |
| `nsfc_youth` | 国自然青年基金 | 无城市比较，重申请人前期基础 |
| `provincial_key` | 省级重点研发 | 重地方产业需求、本地企业参与 |
| `enterprise_rnd` | 企业技术研发 | 重经济效益、知识产权布局、技术成熟度 |

---

## Agent 拓扑与消息总线

所有 Agent 通过 **Dispatcher（调度中枢）** 中转通信，禁止直接点对点通信。

```
User → Dispatcher → Agent-A (解析)
              ↓
         Agent-B (检索)
              ↓
         Agent-C-1/2/3 (并行撰写)
              ↓
         Agent-B (回填)
              ↓
         Agent-D (核查)
              ↓
         Agent-E (组装交付)
              ↓
            User
```

详细状态机见 [prompts/dispatcher.md](./prompts/dispatcher.md)。

---

## 数据流与JSON契约

各 Agent 之间通过严格 JSON 契约交换数据：

- **ParsedTemplate** → 见 [schemas/parsed_template.schema.json](./schemas/parsed_template.schema.json)
- **CitationDatabase** → 见 [schemas/citation_db.schema.json](./schemas/citation_db.schema.json)
- **AuditReport** → 见 [schemas/audit_report.schema.json](./schemas/audit_report.schema.json)

---

## 异常处理与降级策略

| 异常场景 | 处理策略 |
|---|---|
| Agent-B 检索超时（外搜不可用） | 自动降级为 `internal_only`，不影响主流程 |
| Agent-D 评分 < 75 | 自动迭代修复，3轮后输出最佳版本 + 未解决问题清单 |
| 某方向真实文献 < 3 条 | 自动生成"研究空白说明"，不阻塞撰写 |
| Word 生成失败 | 输出 Markdown + JSON，用户手动转换 |
| 人工闸口超时未响应 | 默认 `CONTINUE`（可配置为 `ABORT`） |

---

## 贡献与迭代

- 提交 Issue：反馈 Bug 或新模板需求
- 提交 PR：改进 Agent Prompt 或新增内置模板
- Watch 本仓库：订阅 Skill 版本更新

---

## 许可证

MIT
