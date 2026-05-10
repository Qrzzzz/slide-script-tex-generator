# Template Style Guide

## Design principles

Templates should be:

- simple
- readable
- XeLaTeX-compatible
- suitable for Chinese and English text
- easy to edit manually
- free of absolute local paths
- dependent only on common LaTeX packages

## Default PDF reference

The slide PDF should be referenced through a variable:

```latex
\newcommand{\SlidePDF}{slides.pdf}
```

Slide pages should be inserted with:

```latex
\includegraphics[page=<N>,width=<...>]{\SlidePDF}
```

## Template placeholder

Every template must contain:

```text
%%SLIDE_BLOCKS%%
```

Codex replaces this placeholder with generated slide-script blocks.

## Slide block environments

Use the block environment defined by the selected template:

```latex
% left-thumbnail-clean.tex
\begin{SlideScriptBlock}{Slide 1}{1}
...
\end{SlideScriptBlock}

% top-slide-manuscript.tex
\begin{SlideManuscriptBlock}{Slide 1}{1}
...
\end{SlideManuscriptBlock}

% compact-review-notes.tex
\begin{CompactSlideBlock}{Slide 1}{1}
...
\end{CompactSlideBlock}
```

The second argument is always the 1-based PDF page number.

Put script prose, converted Markdown lists, and verbatim-safe code content in the environment body, not in a third macro argument.

## Page break rule

Every slide block must force a page break.

Each slide and its corresponding script must be treated as one logical unit that cannot be split from the next slide block.

The compact template means visually compact: smaller type, tighter margins, and a smaller slide thumbnail. It does not mean multiple slide blocks may be mixed onto one page.

Do not rely on LaTeX natural page flow to decide where the next slide starts.

Every slide block environment must add `\newpage` internally at the end of the block.

## Template choice

Use `left-thumbnail-clean` when the user wants a normal speaker handout or does not specify a style.

Use `top-slide-manuscript` when the user wants a large slide image and full manuscript.

Use `compact-review-notes` when the user wants compact printable rehearsal notes with one slide-script unit per page.

## Typography

Use `fontspec` and simple fallback logic.

Prefer:

- Times New Roman for Latin text
- Microsoft YaHei, Noto Sans CJK SC, or Source Han Sans SC for Chinese text

Do not assume a specific Chinese font is always installed.

## Avoid

Avoid complex packages, custom class files, shell escape, external image conversion, bibliography systems, TikZ-heavy layouts, and absolute Windows or Unix paths.
