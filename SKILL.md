---
name: slide-script-tex-generator
description: Generate a single standalone LaTeX .tex file that aligns a user-provided slide PDF with a speech script. Use this when the user already has a PDF slide deck and wants a LaTeX speaker handout, rehearsal manuscript, or compact review notes. The skill outputs only LaTeX source and does not convert PPTX, compile LaTeX, run OCR, or create project folders.
---

# Slide Script TeX Generator

## Purpose

Generate one standalone LaTeX source file from:

- a slide deck PDF
- a speech script in Markdown, plain text, txt, or pasted text

The generated `.tex` should display each PDF slide page and place the corresponding speech script beside or below it.

The final answer to the user should contain the complete LaTeX source code.

## Required input

The user must provide a slide PDF.

The default assumed PDF filename is:

```text
slides.pdf
```

If the user's PDF has another filename, use that exact filename in LaTeX, or rename it to `slides.pdf`.

The user may also provide:

- `script.md`
- `script.txt`
- pasted speech text
- number of speakers
- total duration
- desired layout
- language preference

## Filename safety

If the PDF filename contains spaces, underscores, percent signs, non-ASCII path characters, or other LaTeX-sensitive characters, prefer telling the user to rename it to `slides.pdf` unless you are confident the filename can be represented safely.

Never use an absolute path.

## Scope

This skill only generates LaTeX source.

It does not perform file conversion, compilation, OCR, dependency installation, folder generation, or PDF rasterization.

If the user provides PPTX only, respond:

```text
请先把 PPTX 导出为 PDF，再把 PDF 发给我。这个 skill 只处理已经导出的 slide PDF，并输出一个 .tex 文件。
```

## Default assumptions

Use these defaults when the user does not specify preferences:

- layout: `left-thumbnail-clean`
- PDF filename: `slides.pdf`
- script handling: one section per slide when possible
- speaker mode: single speaker
- language mode: preserve the user's original language
- engine target: XeLaTeX
- output: one complete `.tex` source file

## User questions

Ask only for missing information that affects the output.

If layout is unclear, ask the user to choose one:

```text
A. left-thumbnail-clean：左侧 PPT 缩略图，右侧演讲稿，适合正式汇报
B. top-slide-manuscript：上方大图，下方演讲稿，适合排练和背稿
C. compact-review-notes：紧凑打印版，适合复习和节省页数
```

If script splitting is unclear, ask:

```text
你的演讲稿是已经按页分好了，还是需要我按 slides.pdf 页数自动切分？
```

If the user asks for a default, use `left-thumbnail-clean`.

Do not ask about PPTX conversion or LaTeX compilation.

## Script normalization

Normalize the script into slide sections.

Use this priority:

1. Split by `---`
2. Split by headings such as `Slide 1`, `Page 1`, `第1页`, `第 1 页`, `P1`
3. Split by obvious numbered sections
4. If no separator exists, split approximately according to the number of slide pages when the page count is known

Preserve all user content.

If there are fewer script sections than slide pages, create empty placeholders for missing slides.

If there are more script sections than slide pages, keep the extra content under an Extra Notes section at the end of the LaTeX file.

Do not silently delete text.

## Markdown handling

Convert lightweight Markdown into simple LaTeX:

- Convert bullet lists to `itemize` when useful.
- Convert numbered lists to `enumerate` when useful.
- Convert speaker labels such as `Qirui:` or `Speaker A:` to bold labels when possible.
- Treat code fences or code-like snippets as verbatim-style content instead of escaping them into unreadable prose.
- Remove slide/page headings used only as separators from the script body unless they contain meaningful script content.
- Preserve the user's wording and language unless explicitly asked to rewrite or polish.

Escape LaTeX-sensitive characters in slide titles and script text.

## PDF page reference rule

Reference PDF pages directly.

Use:

```latex
\includegraphics[page=1,width=...]{slides.pdf}
```

Do not convert PDF pages to PNG or JPG.

Do not use absolute local paths.

## Template selection

Use one of these templates from `assets/templates/`:

- `left-thumbnail-clean.tex`
- `top-slide-manuscript.tex`
- `compact-review-notes.tex`

Selection rules:

- Use `left-thumbnail-clean.tex` by default.
- Use `top-slide-manuscript.tex` when the user wants larger slide previews or full manuscript rehearsal.
- Use `compact-review-notes.tex` when the user wants compact printable notes.

The templates contain the placeholder:

```text
%%SLIDE_BLOCKS%%
```

Replace it with generated slide-script blocks.

Use the correct block environment for the selected template:

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

The slide/script block body must be written inside the environment body.

Do not put long speech scripts, lists, or code blocks inside LaTeX macro arguments.

If the speech script contains Markdown lists, convert them to `itemize` or `enumerate` and place them directly inside the environment body.

Every slide-script block environment must force a page break through the selected template. Generate slide blocks in order and let the template provide `\newpage`; do not add repeated manual page breaks after generated blocks.

If extra script sections remain after the last PDF page, append them after the generated slide blocks as:

```latex
\section*{Extra Notes}
...
```

## LaTeX compatibility rules

The generated LaTeX must:

- target XeLaTeX
- use relative paths only
- assume the PDF is in the same folder as the `.tex`
- support Chinese and English as much as possible
- use simple common packages
- avoid hard-coded absolute paths
- avoid custom build scripts
- avoid requiring obscure packages
- place user-configurable variables near the top
- escape LaTeX-sensitive characters in normal script text

Escape these characters in normal prose:

```text
& % $ # _ { } ~ ^ \
```

When escaping would damage code-like content, preserve readability and use a safe LaTeX representation such as `\texttt{}` or verbatim where appropriate.

## Post-generation checklist

Before delivering the final `.tex` source, check the common failure points listed in:

```text
references/post-generation-checklist.md
```

The most important check is slide-script alignment:

- PDF page 1 must match the script for slide 1.
- PDF page 2 must match the script for slide 2.
- Continue this one-to-one mapping unless the user explicitly requested another order.

Every slide-script block must force a new page through the selected template. This prevents the next slide preview from being placed after the previous slide's script text and causing slide-script misalignment.

At minimum, verify:

- slide page numbers are continuous
- script sections are in the correct order
- every block uses the environment form
- every block is separated by `\newpage` inside the template
- no script content is lost
- no absolute paths are used
- no raw Markdown remains unintentionally
- LaTeX special characters are escaped
- the final answer contains one complete `.tex` file

## Output format

The final response to the user must include:

1. A short note:
   - the `.tex` assumes the slide PDF is in the same folder
   - the default PDF filename is `slides.pdf`, unless another filename was used
2. The complete LaTeX source code in one code block
3. A short XeLaTeX compile hint as a hint only; do not compile LaTeX:

```text
保存为 main.tex，把 slides.pdf 放在同一目录下，可用 XeLaTeX 编译。本 skill 只输出 .tex 源码，不编译 LaTeX。
```

Do not include build scripts.
Do not create project folders.
Do not compile.
Do not add unrelated explanation.
