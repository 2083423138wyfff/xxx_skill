# 提示词索引

主流程按以下顺序对齐：

1. `dispatcher.md`
2. `file_capability_inspector.md`
3. `intake_agent.md`
4. `reference_material_decomposer.md`
5. `template_analyst.md`
6. `content_analyst.md`
7. `outline_architect.md`
8. `human_gate.md` (`post_outline`, mandatory)
9. `section_writer.md`
   - `section_writer_literature_review.md`
   - `section_writer_research_content.md`
   - `section_writer_team_basis.md`
   - `section_writer_outputs_plan.md`
   - `section_writer_general.md`
10. `figure_prompt_agent.md`
11. `literature_search_backfill.md`
12. `integrator.md`
13. `citation_verifier.md`
14. `full_document_ai_style_auditor.md`
15. `compliance_auditor.md`
16. `delivery_agent.md`
17. `final_file_qa_agent.md`

共享协议：

- `common_protocol.md`
- `agent_prompt_template.md`

## 参考资料拆解链路

`reference_material_decomposer.md` 位于 `Intake Agent` 之后、`Content Analyst` 和 `Outline Architect` 之前，负责生成：

- `SourceSegmentRegistry`
- `SourceSegmentAssemblyPlan`
- 必要时由总控代理生成并路由的 `OutlineRevisionRequest`

旧本子、多主题资料和用户指定片段组合必须先经过该代理拆解。下游正文、内容分析、大纲、合规审查、交付和最终 QA 都必须保留片段级追踪。
