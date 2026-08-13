# 内置模板：企业研发与成果转化类申请书

```yaml
template_id: industry_rnd
title: 企业研发与成果转化类申请书
template_source: built_in
version: 0.1.0
retrieved_at: "2026-08-06"

use_when:
  - 企业内部研发立项、产学研合作、技术开发合同、成果转化、技术转移、产业化示范。
  - 题目强调产品、工艺、市场、客户、知识产权、营收、成本、ROI、技术合同或示范应用。

avoid_when:
  - 以基础科学问题为主，且没有明确产品化、商业化或应用示范目标。
  - 主要内容是社科理论研究或实验室平台建设。

parameter_override_policy:
  priority_order:
    - user_explicit_request
    - user_content_template_ref
    - funding_program_notice
    - built_in_template_default
  rule: 用户明确指定的页数、字数、商业指标、财务口径、章节和输出格式优先于本模板默认值；缺少 ROI、营收、成本、市场规模等数据时不得虚构，只能标记待补充或生成测算框架。

source_basis:
  - authority: 深圳市科技创新局
    year: "2026"
    url: https://stic.sz.gov.cn/xxgk/tzgg/content/post_12809968.html
    rule_status: official
    note: 技术转移和成果转化项目公开申请指南、申请书填报指引、形式审查要点、关联交易说明、阶段性成果佐证材料等附件。
  - authority: 深圳市科技创新局
    year: "2026"
    url: https://stic.sz.gov.cn/gkmlpt/content/12/12754/post_12754043.html
    rule_status: official
    note: 小微企业技术创新项目公开申请指南、申请书模板、考核指标填报说明和可行性研究报告模板。
  - authority: 深圳市科技创新局
    year: "2025"
    url: https://stic.sz.gov.cn/gkmlpt/content/12/12099/post_12099560.html
    rule_status: official
    note: 2025年度小微企业技术创新项目公开可行性研究报告模板、形式审查要点和考核指标填报说明，可用于稳定字段对照。
  - authority: 深圳市科技创新委员会
    year: "2023"
    url: https://www.sz.gov.cn/zfgb/2023/gb1281/content/post_10537105.html
    rule_status: official
    note: 技术转移和成果转化管理办法规定申请材料包括项目申请书、财务审计、专项审计、证书或授牌、科研诚信承诺等。

source_aggregation_policy:
  official_blank_template_available: true
  aggregation_rule: 聚合技术转移成果转化申请书填报指引、小微企业技术创新申请书模板、可行性研究报告模板、考核指标说明和形式审查要点。
  common_sections:
    - 项目背景与应用需求
    - 研发目标与考核指标
    - 技术方案与实施路径
    - 研发基础与团队条件
    - 知识产权与成果转化
    - 市场前景与经济社会效益
    - 风险与保障
  conditional_sections:
    - 技术合同与发票流水
    - 阶段性成果或验收成果佐证
    - 关联交易情况及定价说明
    - 自筹资金承诺
    - 专项审计报告
  conflict_resolution: 当成果转化类和企业技术创新类模板冲突时，优先按用户申报事项选择；缺少财务、合同、客户或市场数据时只保留测算框架和待补资料，不自动填数。

default_constraints:
  total_page_limit: null
  total_word_limit: null
  citation_format: GB/T 7714-2015
  evidence_policy: 市场数据、财务数据、技术合同、知识产权和阶段成果必须有用户资料或可靠来源。
  financial_policy: 缺少财务数据时只写测算口径和待补字段，不填具体金额。
  trl_policy: 技术成熟度可作为分析框架，但等级判断必须有测试、样机、示范或验收材料支撑。
  partner_policy: 供应链、客户和合作伙伴线索必须来自用户资料或公开来源，不得虚构合作关系。

sections:
  - section_id: background_need
    title: 项目背景与应用需求
    required: true
    required_elements:
      - 行业痛点
      - 目标客户或应用场景
      - 政策和市场需求
      - 现有产品或工艺不足
      - 国内外竞品或替代技术对比
      - 技术成熟度分析
    evidence_required: true
    rule_status: inferred

  - section_id: product_objectives
    title: 研发目标与产品指标
    required: true
    required_elements:
      - 产品或服务形态
      - 技术目标
      - 性能指标
      - 质量、成本或效率指标
      - 验收方式
    evidence_required: true
    rule_status: inferred

  - section_id: technical_solution
    title: 技术方案与实施路径
    required: true
    required_elements:
      - 技术路线
      - 关键模块
      - 技术成熟度提升路径
      - 研发计划
      - 里程碑节点
      - 试制、测试或示范方案
      - 风险与应对
    evidence_required: true
    rule_status: inferred

  - section_id: foundation_team
    title: 研发基础与团队条件
    required: true
    required_elements:
      - 企业或团队基础
      - 已有成果
      - 设备、数据、客户或供应链条件
      - 供应链或合作伙伴线索
      - 合作单位分工
    evidence_required: true
    rule_status: inferred

  - section_id: ip_commercialization
    title: 知识产权与成果转化
    required: true
    required_elements:
      - 专利、软著、标准或技术秘密布局
      - 技术合同或转化路径
      - 示范应用
      - 合规性和权属说明
    evidence_required: true
    rule_status: inferred

  - section_id: market_benefit
    title: 市场前景与经济社会效益
    required: true
    required_elements:
      - 市场规模或目标客户
      - 商业模式
      - 收入、成本、利润或ROI测算
      - 产业化路径
      - 社会效益
      - 推广计划
    evidence_required: true
    rule_status: heuristic

audit_focus:
  - 是否虚构市场、财务、客户或合同数据。
  - 技术指标和商业指标是否能验收。
  - 知识产权权属是否清楚。
  - 成果转化路径是否与研发内容一致。
  - TRL等级、供应链和合作伙伴表述是否有证据。
  - 缺少财务数据时是否只输出测算框架而非编造金额。
```
