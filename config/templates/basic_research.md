# 内置模板：基础研究类申请书

```yaml
template_id: basic_research
title: 基础研究类申请书
template_source: built_in
version: 0.1.0
retrieved_at: "2026-08-06"

use_when:
  - 国家自然科学基金面上项目、青年科学基金、地区科学基金等基础研究项目。
  - 省市自然科学基金、校级基础研究、青年启动或种子基金。
  - 题目强调科学问题、作用机制、理论模型、方法学创新、实验验证。

avoid_when:
  - 项目主要目标是产品开发、工程示范、成果转化或平台建设。
  - 申报指南明确要求任务分解、考核指标、经济效益或组织实施方案。
  - 用户只是想写产业化、知识产权或城市对比，不应误套基础研究模板。

parameter_override_policy:
  priority_order:
    - user_explicit_request
    - user_content_template_ref
    - funding_program_notice
    - built_in_template_default
  rule: 用户在启动对齐阶段明确指定的页数、字数、章节、格式、输出格式优先于本模板默认值；如与内置默认值冲突，按用户指定执行，并在 Assumptions 和 audit_report 中记录。

source_basis:
  - authority: 国家自然科学基金委员会
    year: "2026"
    url: https://www.nsfc.gov.cn/p1/3381/2821/99242.html
    rule_status: official
    note: 面上项目和青年科学基金项目C类正文调整为立项依据、研究内容、研究基础三部分，原则上不超过30页。
  - authority: 国家自然科学基金委员会
    year: "2026"
    url: https://www.nsfc.gov.cn/p1/2961/2962/3642/sqs.html
    rule_status: official
    note: 申请书问答强调申请代码、资助类别、AI生成内容核实与声明等要求。
  - authority: 国家自然科学基金委员会
    year: "2026"
    url: https://www.nsfc.gov.cn/p1/3381/2824/99667.html
    rule_status: official
    note: 2026年度项目申请通告要求申请人按各类型项目申请书撰写提纲及相关要求撰写申请书。
  - authority: 中国科学技术大学科研部
    year: "2025"
    url: https://www.ustc.edu.cn/info/1362/22719.htm
    rule_status: inferred
    note: 高校转发填报要求提示正文和预算说明书需下载当年撰写模板，不删减蓝色提纲内容。
  - authority: 喀什大学物理与电气工程学院
    year: "2023"
    url: https://wdy.ksu.edu.cn/info/1371/4407.htm
    rule_status: inferred
    note: 公开国家自然科学类项目申报书撰写提纲，含传统立项依据与研究内容、研究基础等结构。

source_aggregation_policy:
  official_blank_template_available: partially
  aggregation_rule: 优先采用基金委当年官方申请规定和系统模板；无法直接取得系统空白模板时，使用基金委公开申请规定与高校公开转发的模板提纲做交集归纳。
  common_sections:
    - 立项依据
    - 研究内容
    - 研究基础
  conditional_sections:
    - 研究方案及可行性分析
    - 特色与创新之处
    - 年度研究计划及预期成果
    - 预算说明
    - 合作基础和必要性说明
  conflict_resolution: 当2026瘦身提质三段式与旧版五段式提纲冲突时，优先采用用户指定或当年申报系统模板；未指定时按三段式组织，并把旧版细项并入研究内容。

subtype_guidance:
  general_basic_research:
    emphasis:
      - 关键科学问题通常可设置1-3个。
      - 研究内容可按逻辑模块展开。
      - 创新点强调科学价值、理论贡献和方法学突破。
    avoid:
      - 不把产业化状况、城市比较、本地单位线索作为硬性章节。
      - 不把工艺流程、知识产权布局写成硬性要求。
    rule_status: heuristic
  early_career_basic_research:
    emphasis:
      - 科学问题应更聚焦，通常1-2个为宜。
      - 研究内容宜控制为2-3个模块。
      - 强调申请人个人前期研究基础、独立贡献和团队支撑。
      - 篇幅精简，避免把青年项目写成大团队重大项目。
    avoid:
      - 科学问题过大、任务链条过长、指标过度工程化。
    rule_status: heuristic

default_constraints:
  total_page_limit: 30
  total_word_limit: null
  citation_format: GB/T 7714-2015
  evidence_policy: 所有文献、数据、前期成果必须可追溯；无法核实则标记 verification_needed。
  ai_use_policy: 若使用生成式AI辅助整理文献或内容，必须提示用户人工核实并声明。

sections:
  - section_id: rationale
    title: 立项依据
    required: true
    word_limit: null
    page_limit: null
    required_elements:
      - 研究背景与问题来源
      - 国内外研究现状述评
      - 关键科学问题或知识缺口
      - 研究价值与必要性
    forbidden_elements:
      - 未核实的高影响结论
      - 空泛政策口号替代科学问题
    evidence_required: true
    rule_status: official

  - section_id: research_content
    title: 研究内容
    required: true
    word_limit: null
    page_limit: null
    required_elements:
      - 研究目标
      - 研究内容或任务模块
      - 拟解决的关键科学问题
      - 研究思路、方法或技术路线
      - 研究方案可行性分析
      - 特色与创新点
      - 年度研究计划
    forbidden_elements:
      - 把工程产品指标写成科学问题
      - 与立项依据脱节的研究任务
    evidence_required: true
    rule_status: inferred

  - section_id: research_foundation
    title: 研究基础
    required: true
    word_limit: null
    page_limit: null
    required_elements:
      - 前期研究积累
      - 代表性成果及申请人贡献
      - 实验条件或数据基础
      - 团队分工与可行性
    forbidden_elements:
      - 夸大申请人贡献
      - 使用无来源的成果、平台或项目经历
    evidence_required: true
    rule_status: official

audit_focus:
  - 科学问题是否清楚且可研究。
  - 研究内容是否围绕科学问题展开。
  - 前期基础是否真实支撑拟开展研究。
  - 是否存在未经核实的论文、成果、数据或AI生成内容。
  - 是否把基础研究误写成产业化或工程验收项目。
  - 青年类项目是否体现聚焦、可行和申请人个人基础。
```
