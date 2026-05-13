# LaTeX Compatibility Notes

## Markdown conversion
- Convert bullet lists to `itemize`.
- Convert numbered lists to `enumerate`.
- Convert speaker labels (e.g., `Speaker A:`) into bold labels when helpful.

## Escaping
Escape LaTeX-sensitive characters in normal text:
- `# $ % & _ { } ~ ^ \\`

Do not over-escape inside code-style blocks.

## Bilingual content
- Keep original language and wording unless rewrite is requested.
- Preserve Chinese punctuation and English punctuation.

## Block body rule
- Put long script text in environment body.
- Do not place long scripts inside macro arguments.
