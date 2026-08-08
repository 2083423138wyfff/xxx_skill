# 内置模板：哲学社会科学类申请书

```yaml
template_id: social_science
title: 哲学社会科学类申请书
template_source: built_in
version: 0.1.0
retrieved_at: "2026-08-06"

use_when:
  - 国家社科基金、教育部人文社会科学研究项目、省市社科规划项目。
  - 题目强调理论问题、政策问题、社会治理、教育、文化、历史、法学、管理、传播等人文社科议题。
  - 资料以文献综述、研究问题、学科视角、研究方法、前期论文和预期成果为主。

avoid_when:
  - 项目核心是自然科学实验、工程研发、设备平台建设或企业产品立项。

parameter_override_policy:
  priority_order:
    - user_explicit_request
    - user_template_ref
    - funding_program_notice
    - built_in_template_default
  rule: 用户明确指定的字数、页数、匿名要求、章节名称、成果形式和格式优先于本模板默认值；若与内置默认冲突，按用户指定执行并记录假设。

source_basis:
  - authority: 全国哲学社会科学工作办公室
    year: "2026"
    url: https://www.nopss.gov.cn/n1/2026/0506/c431027-40714613.html
    rule_status: official
    note: 国家社科基金年度项目包括一般、重点、青年、西部项目，强调问题意识、学科视角、选题说明和课题论证活页。
  - authority: 教育部社会科学司
    year: "2026"
    url: https://www.moe.gov.cn/s78/A13/tongzhi/202606/t20260612_1440551.html
    rule_status: official
    note: 教育部人文社科一般项目包括规划基金、青年基金、专项任务等，强调自主凝练课题、重点研究方向、网上申报和申请评审书。
  - authority: 全国哲学社会科学工作办公室
    year: "2026"
    url: https://www.nopss.gov.cn/n1/2026/0506/c431027-40714613.html?_refluxos=a10
    rule_status: official
    note: 年度项目公告附件包括2026年度项目申请书、课题论证活页和数据代码表；活页含选题说明，论证字数不超过7000字，选题说明不超过300字。
  - authority: 教育部人文社会科学研究管理平台
    year: "2026"
    url: https://sinoss.moe.edu.cn/indexAction%21to_moreArticle.action
    rule_status: official
    note: 平台通知公告置顶提供2026年教育部人文社科一般项目申请评审书、专项任务项目申请评审书和常见问题释疑下载。
  - authority: 教育部人文社会科学研究管理平台
    year: "2026"
    url: https://sinoss.moe.edu.cn/indexAction%21to_articleView.action?articleId=2c90883c870710ab01870e31be4c1a23
    rule_status: official
    note: 申请评审书下载页提示需解压并阅读填报说明及注意事项。

source_aggregation_policy:
  official_blank_template_available: true
  aggregation_rule: 聚合国家社科基金年度项目申请书/课题论证活页与教育部人文社科申请评审书/B表；匿名评审要求取更严格者。
  common_sections:
    - 选题说明
    - 国内外研究现状述评
    - 研究价值
    - 研究内容
    - 研究思路与方法
    - 重点难点
    - 创新之处
    - 研究基础与预期成果
  conditional_sections:
    - 匿名评审活页或B表
    - 政治方向与意识形态合规说明
    - 经费预算
    - 最终成果形式
    - 阶段性成果
  conflict_resolution: 当国家社科和教育部人文社科模板差异较大时，优先采用用户指定项目；未指定时使用共性课题论证结构，匿名要求按最严格规则执行。

default_constraints:
  total_page_limit: null
  total_word_limit: 7000
  citation_format: GB/T 7714-2015
  anonymity_policy: 若用于匿名评审活页，正文不得出现申请人、团队、单位、已发表成果中的身份线索。
  evidence_policy: 文献综述、政策依据和前期成果必须可追溯。

sections:
  - section_id: topic_statement
    title: 选题说明
    required: true
    word_limit: 300
    required_elements:
      - 核心问题
      - 研究视角
      - 项目定位
      - 选题边界
    evidence_required: false
    rule_status: official

  - section_id: literature_value
    title: 国内外研究现状述评与研究价值
    required: true
    required_elements:
      - 国内外研究现状
      - 主要观点和流派
      - 研究不足
      - 理论价值
      - 实践价值
    evidence_required: true
    rule_status: inferred

  - section_id: content_questions
    title: 研究内容、基本观点与重点难点
    required: true
    required_elements:
      - 研究对象
      - 主要内容
      - 基本观点
      - 重点难点
      - 逻辑框架
    evidence_required: true
    rule_status: inferred

  - section_id: methods_path
    title: 研究思路与研究方法
    required: true
    required_elements:
      - 研究思路
      - 方法体系
      - 资料来源
      - 技术路线或论证路径
      - 可行性
    evidence_required: true
    rule_status: inferred

  - section_id: innovation
    title: 创新之处
    required: true
    required_elements:
      - 理论创新
      - 视角创新
      - 方法创新
      - 资料或案例创新
    evidence_required: true
    rule_status: heuristic

  - section_id: foundation_outputs
    title: 研究基础与预期成果
    required: true
    required_elements:
      - 前期相关成果
      - 研究团队基础
      - 阶段性成果
      - 最终成果
      - 成果转化或咨政应用
    evidence_required: true
    rule_status: inferred

audit_focus:
  - 是否突出问题意识和学科视角。
  - 是否避免把重点研究方向直接当题目。
  - 是否有匿名评审泄密风险。
  - 研究内容、方法和预期成果是否匹配。
```
