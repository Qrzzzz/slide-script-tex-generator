---
name: slide-script-tex-generator
description: Generate a single standalone LaTeX .tex file that aligns a user-provided slide PDF with a speech script. Use this when the user already has a PDF slide deck and wants a LaTeX speaker handout, rehearsal manuscript, or compact review notes. The skill outputs LaTeX source and, when requested, can guide compilation to final PDF using Codex official LaTeX Tectonic tooling. It does not convert PPTX, run OCR, or create project folders.
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

This skill generates LaTeX source and can optionally continue to a compiled PDF workflow.

It does not perform PPTX conversion, OCR, or PDF rasterization.

If the user provides PPTX only, respond:

```text
请先把 PPTX 导出为 PDF，再把 PDF 发给我。这个 skill 只处理已经导出的 slide PDF，并输出一个 .tex 文件。
```

## Default assumptions

Use these defaults when the user does not specify preferences:

- layout: `top-slide-manuscript`
- PDF filename: `slides.pdf`
- script handling: one section per slide when possible
- speaker mode: single speaker
- language mode: preserve the user's original language
- engine target: XeLaTeX
- output: one complete `.tex` source file


## PDF-first completion workflow

After producing `main.tex`, ask whether the user wants to continue to a compiled PDF.

If yes, prefer Codex official LaTeX Tectonic plugin/tooling.

Recommended flow:

1. Confirm whether official Tectonic support is already available in the user's Codex environment.
2. If not available, guide the user to install/enable the official LaTeX Tectonic plugin first (or install it on their behalf when the environment allows).
3. Compile `main.tex` to `main.pdf` with Tectonic.
4. Report compile status and obvious LaTeX errors with actionable fixes.

When the environment cannot compile, still provide exact commands the user can run locally.

## Required pre-generation questions

Before generating the final `.tex`, ask the user the following questions unless the answer has already been clearly provided:

1. 是否使用默认版式 `top-slide-manuscript`？

Explain this default layout briefly:

`top-slide-manuscript` means: each output page shows a large slide preview on the top and the corresponding speech manuscript below it. It is best for rehearsal, reading, and slide-by-slide manuscript alignment.

Chinese explanation:

`top-slide-manuscript` 是默认版式：每一页上方放当前 PPT 页的大图，下方放对应这一页的演讲稿。它适合排练、背稿和逐页对照检查。

2. 演讲稿是否已经按 PPT 页码分好？

Ask:

- 已经用 `---` 分好了
- 已经用 `Slide 1 / 第1页` 之类标题分好了
- 没有分好，需要按 PDF 页数大致切分

3. 是否允许自动调整每页演讲稿字号？

Ask:

- 允许：短稿页面可以适当放大字号
- 不允许：全部页面使用统一字号

Default: allow adaptive font sizing.

4. 如果 PDF 中可能存在空白页或过渡页，应该如何处理？

Ask:

- 保留空白页，并生成空白演讲稿占位
- 跳过空白页
- 让我手动指定哪些页是空白页或不需要讲稿

Default: do not guess. Ask user first if blank pages are detected or suspected.

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

## Blank page handling

If the slide PDF appears to contain blank pages, transition pages, section dividers, or pages with no meaningful visible content, do not silently generate the final `.tex`.

Ask the user how to handle them.

The skill should present options:

1. Keep these pages and create empty script placeholders.
2. Skip these pages.
3. Let the user manually specify which pages to include or exclude.

If the system cannot inspect the PDF visually, but the script section count and slide count strongly suggest possible blank or divider pages, ask the user before generation.

Examples of mismatch that should trigger a question:

- PDF has more pages than script sections.
- Script has obvious slide numbers but skips pages.
- There are very short or empty script sections between normal sections.
- The user mentions title pages, transition pages, blank pages, divider slides, or backup slides.

Do not assume blank pages should be removed.

## Adaptive manuscript font sizing

When the selected template is `top-slide-manuscript`, the skill may adapt the manuscript font size per slide if the user allows it.

Purpose:

- If a slide has very little script content, make the text larger so the page looks fuller and easier to read.
- If a slide has a normal or long script, use a normal readable size.
- Never make text so large that it risks being cut off, pushed outside the page, or causing severe overflow.

Use conservative size levels only:

- very short script: `\Large`
- short script: `\large`
- normal script: `\normalsize`
- long script: `\small`

Do not use extremely large sizes such as `\LARGE`, `\huge`, or `\Huge` for manuscript body text.

Recommended heuristic:

- 0–60 Chinese characters or 0–45 English words: `\Large`
- 61–140 Chinese characters or 46–100 English words: `\large`
- 141–350 Chinese characters or 101–230 English words: `\normalsize`
- more than that: `\small`

If unsure, choose the smaller size.

The adaptive font size should be passed into the slide block environment or defined per block in a stable way.

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

- Use `top-slide-manuscript.tex` by default.
- Use `left-thumbnail-clean.tex` when the user wants a classic left-slide right-script handout.
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
