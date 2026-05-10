# Post-generation Checklist

Use this checklist before delivering the final `.tex` source to the user.

## 1. Slide-script alignment

- Check that `page=1` corresponds to the script for slide 1.
- Check that `page=2` corresponds to the script for slide 2.
- Continue this check for all slides.
- PDF page numbers must be continuous and monotonic: `1, 2, 3, ...`.
- Do not skip, repeat, or reorder PDF page numbers unless the user explicitly requested it.

## 2. Forced page break

- Every slide-script block must end with a forced page break.
- The template should provide `\newpage` at the end of each block environment.
- Do not rely on natural page flow to separate slides.
- A new slide must start on a new page.

## 3. Script section count

- Count the number of slide pages when known.
- Count the number of script sections.
- If there are fewer script sections than slides, add empty placeholders.
- If there are more script sections than slides, preserve extra content under `Extra Notes`.
- Never delete script content silently.

## 4. Environment block format

Use environment form:

```latex
\begin{SlideScriptBlock}{Slide 1}{1}
...
\end{SlideScriptBlock}
```

Do not use macro-body form:

- Do not write a slide block as one command with the script placed in a third braced argument.
- Do not put long script text, lists, or code-like content inside macro arguments.

Long script text, lists, and code-like content must be placed in the environment body.

## 5. PDF filename and path

- Use a relative path.
- Default filename is `slides.pdf`.
- Do not use absolute paths.
- Do not use local temporary paths.
- If the user's PDF filename is different, use the exact filename consistently.

## 6. LaTeX escaping

Escape special characters in normal text:

```text
& % $ # _ { } ~ ^ \
```

Examples:

```latex
DeepSeek\_V4
\%
```

Do not leave raw `%` in prose.

## 7. Markdown conversion

Convert Markdown syntax into LaTeX:

- Markdown bullet lists become `itemize`.
- Markdown numbered lists become `enumerate`.
- `**bold**` becomes `\textbf{bold}`.
- Inline code becomes `\texttt{code}`.
- Do not paste raw Markdown into LaTeX unless it is intentionally shown as text.

## 8. Font and language compatibility

- The document should target XeLaTeX.
- Keep Chinese font fallback logic.
- Preserve Chinese and English text.
- Do not hard-code one non-universal Chinese font as the only option.

## 9. Skill scope

Confirm that the generated answer does not introduce:

- PPTX conversion
- running LaTeX compilation
- OCR
- build scripts
- generated folders
- slide image rasterization

The only formal output is one complete `.tex` source file.

## 10. Final response format

The final answer should include:

- A short note about the expected slide PDF filename.
- One complete LaTeX source code block.
- A short XeLaTeX compile hint as a user-facing hint only; do not compile.

Do not return partial snippets only.
