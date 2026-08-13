# 内置模板：科研平台与载体建设类申请书

```yaml
template_id: platform_construction
title: 科研平台与载体建设类申请书
template_source: built_in
version: 0.1.0
retrieved_at: "2026-08-06"

use_when:
  - 重点实验室、工程研究中心、临床医学研究中心、概念验证中心、中试基地、创新载体建设。
  - 题目强调平台定位、研究方向、建设基础、队伍、仪器条件、开放服务、运行机制。

avoid_when:
  - 只是单个科研课题，没有平台建设、运行管理或开放服务要求。
  - 企业只需要产品研发或成果转化方案。

parameter_override_policy:
  priority_order:
    - user_explicit_request
    - user_content_template_ref
    - funding_program_notice
    - built_in_template_default
  rule: 用户明确指定的建设周期、页数、字数、章节、平台类型和格式优先于本模板默认值；若缺少场地、设备、人员、经费等硬性建设条件，不得补造，只列为待补资料。

source_basis:
  - authority: 深圳市科技创新局
    year: "2026"
    url: https://stic.sz.gov.cn/zxbs/bszn/index.html
    rule_status: official
    note: 办事指南列出重点实验室组建资助、概念验证中心、中小试基地等平台和载体专项及申请指南、填报指引、形式审查要点。
  - authority: 上海市发展和改革委员会
    year: "2019"
    url: https://fgw.sh.gov.cn/fgw_gjscy/20211101/3cf0edd8d4584433839bd5489c308c70.html
    rule_status: official
    note: 上海市工程研究中心申请报告编制提纲包含摘要、建设必要性、申报单位条件、主要任务目标、管理运行机制等。
  - authority: 教育部
    year: "2019"
    url: https://kjc.ecnu.edu.cn/_t1493/7e/a0/c8591a97952/page.htm
    rule_status: official
    note: 教育部工程研究中心建设申请书编制大纲包含建设意义、申报单位概况、主要任务目标、管理运行机制等。
  - authority: 深圳市科技创新局
    year: "2026"
    url: https://stic.sz.gov.cn/gkmlpt/content/12/12809/post_12809327.html
    rule_status: official
    note: 深圳市重点实验室组建资助项目公开申请指南、申请书填报指引、形式审查要点和可行性研究报告模板。
  - authority: 深圳市科技创新局
    year: "2024"
    url: https://www.sz.gov.cn/zfgb/2024/gb1354/content/post_11904463.html
    rule_status: official
    note: 深圳市重点实验室管理办法规定重点实验室定位目标、研究方向、固定人员、科研用房、仪器设备、持续保障等条件。
  - authority: 深圳市科技创新局
    year: "2026"
    url: https://stic.sz.gov.cn/zxbs/bszn/index.html
    rule_status: official
    note: 办事指南还列出概念验证中心、中小试基地认定与评估的申请指南、形式审查要点、专项审计要点和费用清单模板。

source_aggregation_policy:
  official_blank_template_available: true
  aggregation_rule: 聚合工程研究中心建设申请书编制大纲、重点实验室组建申请书填报指引、可行性研究报告模板、概念验证中心和中小试基地认定材料要求。
  common_sections:
    - 摘要
    - 建设背景与必要性
    - 申报单位概况与建设基础
    - 研究方向、主要任务与目标
    - 建设内容与条件保障
    - 管理与运行机制
    - 投资或经费情况
    - 经济社会效益
    - 附件材料
  conditional_sections:
    - 学术委员会或专家委员会
    - 固定人员清单
    - 仪器设备清单
    - 场地证明
    - 联合建设协议
    - 伦理与科技安全材料
    - 专项审计或费用清单
  conflict_resolution: 当实验室、工程中心、概念验证中心和中小试基地要求不同，优先按用户平台类型和官方 template_ref；未指定时使用平台建设共性结构，并把硬性条件列为待补核验项。

default_constraints:
  total_page_limit: null
  total_word_limit: null
  citation_format: GB/T 7714-2015
  evidence_policy: 平台条件、人员队伍、仪器设备、场地、成果和经费必须有资料支撑。
  construction_policy: 建设目标必须拆成可验收的能力、服务、成果和管理指标。

sections:
  - section_id: summary
    title: 摘要
    required: true
    required_elements:
      - 平台名称
      - 建设单位
      - 建设目标
      - 研究方向
      - 预期能力
    evidence_required: false
    rule_status: official

  - section_id: necessity
    title: 建设背景与必要性
    required: true
    required_elements:
      - 领域地位与需求
      - 国内外发展状况
      - 关键共性技术或服务短板
      - 建设意义
    evidence_required: true
    rule_status: official

  - section_id: foundation
    title: 申报单位概况与建设基础
    required: true
    required_elements:
      - 依托单位概况
      - 学科或产业基础
      - 人才队伍基础
      - 代表性成果
      - 现有场地、设备和条件
    evidence_required: true
    rule_status: official

  - section_id: directions_tasks
    title: 研究方向、主要任务与目标
    required: true
    required_elements:
      - 平台定位
      - 主要研究方向
      - 近期目标
      - 中长期目标
      - 服务对象或开放共享目标
    evidence_required: true
    rule_status: official

  - section_id: construction_plan
    title: 建设内容与条件保障
    required: true
    required_elements:
      - 仪器设备建设
      - 场地和信息化条件
      - 经费投入和配套
      - 建设进度
      - 验收指标
    evidence_required: true
    rule_status: inferred

  - section_id: operation
    title: 管理与运行机制
    required: true
    required_elements:
      - 组织架构
      - 学术委员会或专家委员会
      - 人才引进与培养
      - 开放合作
      - 运行管理和绩效评价
    evidence_required: true
    rule_status: official

audit_focus:
  - 建设基础是否足以支撑平台目标。
  - 人员、设备、场地、经费是否有证据。
  - 研究方向和开放服务是否匹配平台定位。
  - 验收指标是否可量化、可检查。
```
