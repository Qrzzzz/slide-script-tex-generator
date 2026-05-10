# LaTeX Compatibility Notes

## Engine

The generated document targets XeLaTeX.

## File placement

The generated `.tex` and the slide PDF should be in the same folder.

Recommended structure:

```text
slides.pdf
main.tex
```

## Filename safety

Prefer `slides.pdf` for maximum compatibility.

If the original PDF filename contains spaces, underscores, percent signs, shell-sensitive characters, or non-ASCII path characters, ask the user to rename it to `slides.pdf` unless the filename can be represented safely in LaTeX.

## Fonts

The templates use `fontspec`.

They attempt to use:

- Times New Roman
- Microsoft YaHei
- Noto Sans CJK SC
- Source Han Sans SC

The generated source keeps font names near the top so users can replace them later if their LaTeX environment lacks the defaults.

## Compilation boundary

The skill does not run LaTeX compilation, create build scripts, or manage a LaTeX project. It only outputs one `.tex` source file.

## PDF page references

Slides are referenced directly from the PDF:

```latex
\includegraphics[page=1,width=0.4\textwidth]{slides.pdf}
```

No slide images are generated.

## Text escaping

Escape LaTeX-sensitive characters in normal prose and slide titles:

```text
& % $ # _ { } ~ ^ \
```

For code-like content, use `verbatim`, `\texttt{}`, or another readable safe representation rather than aggressively escaping everything inline.
