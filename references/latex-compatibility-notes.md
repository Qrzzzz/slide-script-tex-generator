# LaTeX Compatibility Notes

## Markdown conversion
- Convert bullet lists to `itemize`.
- Convert numbered lists to `enumerate`.
- Convert speaker labels (e.g., `Speaker A:`) into bold labels when helpful.

## LaTeX special character mapping

| Character | Escape form |
|---|---|
| `#` | `\#` |
| `$` | `\$` |
| `%` | `\%` |
| `&` | `\&` |
| `_` | `\_` |
| `{` | `\{` |
| `}` | `\}` |
| `~` | `\textasciitilde{}` |
| `^` | `\textasciicircum{}` |
| `\` | `\textbackslash{}` |

## Code blocks, paths, and commands
- Do not mechanically escape code-like text into unreadable prose.
- Use `verbatim` style or `\texttt{...}` for commands and path examples.
- Apply escaping mainly to normal prose content.

## Bilingual content
- Keep original language and wording unless rewrite is requested.
- Preserve Chinese punctuation and English punctuation.

## Block body rule
- Put long script text in environment body.
- Do not place long scripts inside macro arguments.
