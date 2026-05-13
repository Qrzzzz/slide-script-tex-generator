---
name: slide-script-tex-generator
description: Generate one complete main.tex by aligning slides.pdf pages with per-slide scripts. Use when users need editable LaTeX speaker notes or rehearsal manuscripts from an exported slide PDF.
---

# Slide Script TeX Generator Skill Spec

## 1) Purpose

Generate one complete `main.tex` from:
- `slides.pdf`
- per-slide script text (Markdown/plain text/pasted)

Normal final answer: **one complete `main.tex` source file**.

## 2) Required inputs

- Required: exported slide PDF (default filename `slides.pdf`)
- Optional: script file/text, layout preference, language preference

If user only provides PPTX: ask them to export PDF first.

## 3) Defaults

- Layout: `top-slide-manuscript`
- Output filename target: `main.tex`
- Slide reference: `\newcommand{\SlidePDF}{slides.pdf}`
- Language handling: preserve user language
- Script mapping: one section per slide when possible

## 4) Required pre-generation questions

Ask these unless already answered:
1. Use default layout `top-slide-manuscript`?
2. How is script split (`---`, slide headings, or unsplit)?
3. Allow adaptive font sizing (top-slide-manuscript only)?
4. How to handle blank/divider pages (keep, skip, or manual page list)?

## 5) Script normalization

Priority:
1. Split by `---`
2. Split by headings (`Slide 1`, `Page 1`, `第1页`, etc.)
3. Split by numbered sections
4. Otherwise split approximately by slide count

Rules:
- Keep all user content.
- If script sections are fewer than slides, create empty placeholders.
- If script sections exceed slides, append extras under `Extra Notes`.
- Do not silently delete text.
- Convert lightweight Markdown to LaTeX (`itemize`, `enumerate`, emphasis).
- Escape LaTeX-sensitive characters in text content.

## 6) Template selection

Select from `assets/templates/`:
- `top-slide-manuscript.tex` (default)
- `left-thumbnail-clean.tex`
- `compact-review-notes.tex`

All templates must:
- include `%%SLIDE_BLOCKS%%`
- use `\newcommand{\SlidePDF}{slides.pdf}`
- avoid absolute paths

Environment names:
- `SlideScriptBlock`
- `SlideManuscriptBlock`
- `CompactSlideBlock`

## 7) LaTeX generation rules

- Reference PDF pages directly with `\includegraphics[page=N,...]{\SlidePDF}`.
- Generate blocks in slide order.
- End each slide block with a new page (via template environment design).
- Keep long script content in environment body, not macro arguments.
- Keep bilingual text intact; ensure Chinese/English compatibility.
- Adaptive font sizing is optional and only for `top-slide-manuscript`.

## 8) Optional compilation boundary

**Do not compile unless explicitly requested and environment supports it.**

Core skill completion is `main.tex`.
If user requests compilation, provide or run:
- Default local command: `xelatex main.tex` twice
- Optional command: `tectonic main.tex`

## 9) Post-generation checklist

Follow `references/post-generation-checklist.md` before final output.

## 10) Output format

- Return one complete LaTeX document source (`main.tex` content).
- No partial snippets unless user explicitly asks for partial edits.
