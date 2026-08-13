# DOCX 格式模板选择协议

## 固定决策链

```text
用户给模板
  -> 严格抽取模板格式
  -> 按用户模板生成

用户没给模板
  -> 展示内置模板格式
  -> 问用户是否同意

用户同意
  -> 使用内置模板格式

用户不同意
  -> 问用户是否有模板提供

用户提供
  -> 回到“严格抽取模板格式”

用户不提供
  -> 逐项询问字体、字号、行距、页边距、标题、表格、图题、页眉页脚等
```

## 硬规则

- 用户提供 DOCX 模板时，必须以用户模板为格式权威。
- 用户模板必须被结构化抽取为 `DocxFormatProfile`，不得凭肉眼印象或通用 Word 默认值生成。
- 用户未提供 DOCX 模板时，必须先展示内置格式模板摘要，并询问用户是否同意使用。
- 用户未明确同意前，不得使用内置格式模板。
- 用户拒绝内置格式模板后，必须询问用户是否能提供模板。
- 用户拒绝内置格式模板且不能提供模板时，必须逐项询问格式参数。
- 不得把“用户沉默”解释为同意内置模板。

## 逐项询问字段

```yaml
manual_docx_format_questions:
  - page_size
  - orientation
  - cover_page_margins
  - body_page_margins
  - header_distance
  - footer_distance
  - body_chinese_font
  - body_western_font
  - body_font_size
  - body_line_spacing
  - body_alignment
  - first_line_indent
  - paragraph_spacing_before
  - paragraph_spacing_after
  - heading_1_font
  - heading_1_size
  - heading_1_spacing
  - heading_2_font
  - heading_2_size
  - heading_2_spacing
  - heading_3_font
  - heading_3_size
  - toc_style
  - figure_caption_style
  - table_caption_style
  - table_body_style
  - table_border_style
  - header_text
  - footer_page_number_style
  - numbering_style
```

## 产物要求

```yaml
docx_format_profile:
  source: user_template | builtin_template | manual_user_answers
  source_ref: string
  user_approved_builtin_template: true | false
  extracted_at: ISO8601
  page_setup: {}
  body_style: {}
  heading_styles: {}
  toc_style: {}
  figure_style: {}
  table_style: {}
  header_footer_style: {}
  numbering_style: {}
  unresolved_format_questions: []
```

`Delivery Agent` 只能使用 `docx_format_profile` 中已经确认的格式规则生成 DOCX。
