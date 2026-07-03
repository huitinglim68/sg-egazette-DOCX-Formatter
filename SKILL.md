---
name: egazette-docx-formatter
description: Formats final e-Gazette rows for DOCX export with clickable reference markers in the Summary column. Use this skill when you have an ordered list of final notice data and need to prepare a 5-column table for DOCX output.
---

# e-Gazette DOCX Formatter

This skill provides instructions for preparing a final table of e-Gazette notices specifically for DOCX export. It ensures that reference links are preserved as clickable markers within the Summary field.

## Input Data Schema

The skill expects an ordered list of rows, where each row contains:
- `entity_name`: The name of the company or entity.
- `summary_text`: The descriptive text for the notice.
- `identifier`: The unique ID or reference code for the notice.
- `publication_date`: When the notice was published.
- `team`: The assigned team.
- `references`: An array of objects, each containing `{ reference_number, pdf_url }`.

## Formatting Instructions

1.  **Maintain Order**: Keep the rows in the exact order provided in the input.
2.  **Column Structure**: Produce exactly 5 columns in this order:
    - Entity/Company Name
    - Summary
    - Identifier
    - Publication Date
    - Team
3.  **Summary Field Construction**:
    - Start with the original `summary_text`.
    - Do NOT rewrite or modify the `summary_text`.
    - Append reference markers at the end of the text.
    - Each marker must be shown as `[N]` (e.g., `[1]`, `[2]`).
    - **Crucial**: Each `[N]` must be a clickable hyperlink pointing to its corresponding `pdf_url`.
4.  **No Raw URLs**: Do not display raw URLs in the table unless hyperlink formatting is impossible in the target environment.

## Output Template

| Entity/Company Name | Summary | Identifier | Publication Date | Team |
| :--- | :--- | :--- | :--- | :--- |
| [entity_name] | [summary_text] [[1]](pdf_url_1) [[2]](pdf_url_2) | [identifier] | [publication_date] | [team] |

## Example

**Input:**
```json
[
  {
    "entity_name": "ACME CORP PTE LTD",
    "summary_text": "Winding up order notice issued.",
    "identifier": "2024-00123",
    "publication_date": "2024-06-17",
    "team": "Team A",
    "references": [
      { "reference_number": 1, "pdf_url": "https://example.com/notice1.pdf" },
      { "reference_number": 2, "pdf_url": "https://example.com/notice2.pdf" }
    ]
  }
]
```

**Output:**

| Entity/Company Name | Summary | Identifier | Publication Date | Team |
| :--- | :--- | :--- | :--- | :--- |
| ACME CORP PTE LTD | Winding up order notice issued. [[1]](https://example.com/notice1.pdf) [[2]](https://example.com/notice2.pdf) | 2024-00123 | 2024-06-17 | Team A |

## Quality Checklist

- [ ] Row order is identical to the input.
- [ ] Each `[N]` points to the correct `pdf_url`.
- [ ] Exactly 5 columns are present.
- [ ] Summary text is preserved exactly as provided.
