# 内置模板：通用简版申请书

```yaml
template_id: compact_proposal
title: 通用简版申请书
template_source: built_in
version: 0.1.0
retrieved_at: "2026-08-06"

use_when:
  - 用户没有提供模板，且无法可靠判断项目属于基础研究、任务研发、社科、企业转化或平台建设。
  - 用户只需要预申请、内部讨论稿、校级种子基金、概念框架或待补资料清单。
  - 启动对齐阶段资料不足，但用户明确接受假设版框架或简版草稿。

avoid_when:
  - 用户提供了明确官方模板或申报指南。
  - 用户要求生成可直接提交的正式申请书，但关键资料不足。

parameter_override_policy:
  priority_order:
    - user_explicit_request
    - user_content_template_ref
    - funding_program_notice
    - built_in_template_default
  rule: 用户明确指定的页数、字数、章节和输出格式优先于本模板默认值；如果资料不足，只允许输出框架、资料清单或假设版草稿，并显式标注 Assumptions。

source_basis:
  - authority: xxx-skill
    year: "2026"
    url: internal
    rule_status: heuristic
    note: 由基础研究、重点研发、社科、企业转化和平台建设模板的交集归纳，用作低信息输入时的兜底框架，不代表任何官方申报格式。

source_aggregation_policy:
  official_blank_template_available: false
  aggregation_rule: 该模板没有官方空白模板来源，只用于信息不足时的通用框架；一旦用户提供官方模板、申报指南或明确类型，应立即切换到对应模板族。
  common_sections:
    - 背景与意义
    - 目标与拟解决问题
    - 研究内容与实施路径
    - 创新点或特色
    - 基础条件与预期成果
  conditional_sections:
    - 待补资料清单
    - 假设清单
    - 用户确认问题
    - 国内外现状简述
    - 技术路线或研究思路
    - 经济社会效益
  conflict_resolution: 不与官方模板竞争；任何用户指定或官方模板规则都优先于 compact_proposal。

default_constraints:
  total_page_limit: null
  total_word_limit: 3000
  citation_format: GB/T 7714-2015
  evidence_policy: 无用户资料支撑的事实、数据、单位、成果和引用不得写成确定事实。
  readiness_policy: readiness 非 ready 时，交付物必须包含 assumptions.md 和 missing_materials.md。

sections:
  - section_id: background
    title: 背景与意义
    required: true
    required_elements:
      - 问题背景
      - 需求或研究价值
      - 现有不足
    evidence_required: true
    rule_status: heuristic

  - section_id: objectives
    title: 目标与拟解决问题
    required: true
    required_elements:
      - 总体目标
      - 具体目标
      - 拟解决的关键问题
    evidence_required: false
    rule_status: heuristic

  - section_id: content_route
    title: 研究内容与实施路径
    required: true
    required_elements:
      - 研究内容或任务模块
      - 方法路线
      - 阶段计划
      - 可行性说明
    evidence_required: true
    rule_status: heuristic

  - section_id: innovation
    title: 创新点或特色
    required: true
    required_elements:
      - 主要创新
      - 与现有工作的差异
      - 预期突破
    evidence_required: true
    rule_status: heuristic

  - section_id: foundation_outputs
    title: 基础条件与预期成果
    required: true
    required_elements:
      - 已有基础
      - 团队或资源条件
      - 预期成果
      - 待补资料
    evidence_required: true
    rule_status: heuristic

audit_focus:
  - 是否清楚标注所有假设。
  - 是否避免伪装成正式官方模板。
  - 是否列出下一步补资料清单。
  - 是否存在凭空编造的成果、数据、单位或引用。
  - 是否提醒用户后续应切换到明确模板族或上传官方模板。
```
