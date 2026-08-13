# 内置模板：任务牵引研发类申请书

```yaml
template_id: mission_rnd
title: 任务牵引研发类申请书
template_source: built_in
version: 0.1.0
retrieved_at: "2026-08-06"

use_when:
  - 国家重点研发计划、省市重点研发计划、技术攻关、揭榜挂帅、重大专项。
  - 题目强调关键核心技术、应用示范、工程系统、考核指标、任务分解。
  - 用户资料包含指南方向、榜单任务、技术指标、牵头单位和参与单位。
  - 地方重点研发计划中强调本省市产业需求、产学研合作、本地示范和区域对比。

avoid_when:
  - 项目只关注基础科学问题，没有明确工程目标或考核指标。
  - 项目核心是社科论证、平台建设或企业内部商业立项。

parameter_override_policy:
  priority_order:
    - user_explicit_request
    - user_content_template_ref
    - funding_program_notice
    - built_in_template_default
  rule: 用户明确指定的页数、字数、章节、考核指标、研究周期和格式优先于本模板默认值；若用户指定与内置默认冲突，按用户指定执行，并记录冲突来源。

source_basis:
  - authority: 科学技术部国家科技管理信息系统公共服务平台
    year: "2021"
    url: https://service.most.gov.cn/wjfg_zdyf/
    rule_status: official
    note: 平台公开国家重点研发计划项目申报书、课题任务书、青年科学家项目申报书等模板条目。
  - authority: 科学技术部国家科技管理信息系统公共服务平台
    year: "2021"
    url: https://service.most.gov.cn/wjfg_zdyf/20211029/4683.html
    rule_status: official
    note: 国家重点研发计划青年科学家项目申报书模板公开下载页。
  - authority: 科学技术部
    year: "2021"
    url: https://service2.most.gov.cn/kjjh_tztg_all/20210329/4236.html
    rule_status: official
    note: 重点专项指南通知说明项目可下设课题，须覆盖指南方向考核指标，预申报书约3000字，说明目标指标、创新思路、技术路线和研究基础。
  - authority: 国家自然科学基金委员会
    year: "2026"
    url: https://service.most.gov.cn/kjjh_tztg_all/20260720/5842.html
    rule_status: official
    note: 2026年部分重点专项采用一轮申报，项目可下设课题，申报书附件材料全部电子扫描上传，并附形式审查要点。
  - authority: 中关村绿色矿山产业联盟
    year: "2016"
    url: https://greenmine.org.cn/shows/30/28544.html
    rule_status: inferred
    note: 公开转录重点研发项目申报书提纲，包含指标、研究内容、研究方法与技术路线、任务分解、创新点、经济社会效益等。

source_aggregation_policy:
  official_blank_template_available: true
  aggregation_rule: 以科技部国科管系统公开的项目申报书、预申报书、课题任务书、青年科学家项目模板为主，结合当年专项指南和形式审查要求抽取共性章节。
  common_sections:
    - 项目概况
    - 目标与考核指标
    - 研究内容
    - 研究方法与技术路线
    - 任务或课题分解
    - 创新点
    - 研究基础与团队条件
    - 进度安排与经费预算
  conditional_sections:
    - 预申报书3000字摘要
    - 联合申报协议
    - 诚信承诺书
    - 青年科学家项目个人基础
    - 中期指标
    - 经济社会效益
    - 地方产业需求对接
    - 区域或城市比较
    - 本地单位、企业或科研院所线索
    - 知识产权规划
  conflict_resolution: 当项目申报书、课题申报书、青年科学家模板章节不同，优先匹配用户的 funding_program 和 template_type；无法判断时使用项目级共性结构，并把课题级内容作为任务分解子表。

source_pattern_migration:
  source_patterns:
    - national_mission_rnd
    - provincial_mission_rnd
  keep_as_conditional_rules:
    - 国内外现状可按用户资料中的研究方向、任务或技术领域动态分节。
    - 国外现状可包含代表性机构、技术特点、代表性成果、产业化状况、最新进展和趋势。
    - 国内现状可包含代表性机构、技术特点、代表性成果、产业化状况和发展趋势。
    - 地方项目可加入本省市产业需求、区域对比和本地单位线索。
    - 技术路线部分可包含技术领域、工艺范畴、关键技术问题、技术原理、技术方法、工艺流程、创新点和知识产权规划。
  generalization:
    - 历史规则中的固定城市比较改为 region_comparison，由用户申报地区决定。
    - 历史规则中的固定本地单位线索改为 local_partner_mapping，由用户所在地和资料决定。
    - 历史规则中的固定子方向改为按 ContentOutline.directions 动态分节。
    - 历史规则中的固定闭环示例只作为用户资料匹配时的表达方式，不得硬编码到所有项目。
  rule_status: heuristic

default_constraints:
  total_page_limit: null
  total_word_limit: null
  citation_format: GB/T 7714-2015
  evidence_policy: 国内外现状、技术成熟度、考核指标和示范基础必须绑定来源。
  indicator_policy: 所有指标应尽量量化；无法量化时说明验收方式。
  direction_policy: 如用户资料包含多个研究方向、课题或任务，按实际数量动态分节；不得默认5个方向。
  region_policy: 只有在用户指定地区、申报指南要求地方特色，或资料中存在区域需求时，才写区域比较和本地单位线索。

sections:
  - section_id: overview
    title: 项目概况
    required: true
    required_elements:
      - 项目名称
      - 所属专项或指南方向
      - 牵头单位与参与单位
      - 项目负责人
      - 研究周期
    evidence_required: false
    rule_status: inferred

  - section_id: background
    title: 立项依据与国内外现状
    required: true
    required_elements:
      - 国家或行业需求
      - 国内外技术现状
      - 代表性机构与成果
      - 产业化或应用进展
      - 关键瓶颈
      - 现有基础与差距
      - 地方产业需求对接
    evidence_required: true
    rule_status: inferred

  - section_id: objectives_indicators
    title: 研究目标与考核指标
    required: true
    required_elements:
      - 总体目标
      - 分阶段目标
      - 技术指标
      - 应用示范或验收指标
    evidence_required: true
    rule_status: inferred

  - section_id: research_tasks
    title: 研究内容与任务分解
    required: true
    required_elements:
      - 任务或课题分解
      - 每项任务的研究内容
      - 单位分工
      - 任务间依赖关系
    evidence_required: true
    rule_status: inferred

  - section_id: technical_route
    title: 技术路线与实施方案
    required: true
    required_elements:
      - 技术路线
      - 技术领域与工艺范畴
      - 关键技术问题
      - 技术原理与技术方法
      - 关键技术方案
      - 工艺流程或系统集成流程
      - 试验验证方案
      - 系统集成或示范方案
      - 风险与应对
    evidence_required: true
    rule_status: inferred

  - section_id: innovation
    title: 创新点
    required: true
    required_elements:
      - 理论创新
      - 方法创新
      - 技术创新
      - 工程应用创新
      - 知识产权或标准规划
    evidence_required: true
    rule_status: heuristic

  - section_id: team_foundation
    title: 研究基础与团队条件
    required: true
    required_elements:
      - 前期项目和成果
      - 牵头单位基础
      - 参与单位分工
      - 平台和数据条件
    evidence_required: true
    rule_status: inferred

  - section_id: schedule_budget
    title: 进度安排与经费预算
    required: true
    required_elements:
      - 年度或阶段计划
      - 里程碑
      - 经费测算
      - 组织管理机制
    evidence_required: true
    rule_status: inferred

audit_focus:
  - 目标、任务、技术路线、考核指标是否一一对应。
  - 任务分工是否闭环，是否存在无人负责的内容。
  - 指标是否可量化、可验收、可追溯。
  - 是否误把规划性描述写成已完成成果。
  - 区域比较和本地单位线索是否有用户资料或可靠来源支撑。
  - 章节是否按实际任务或方向动态拆分，而不是沿用固定5方向。
```
