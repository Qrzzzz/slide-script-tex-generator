---
name: slide-script-tex-generator
description: Generate a compiled PDF speaker handout by aligning slides.pdf pages with per-slide scripts. The skill first creates main.tex as an intermediate build artifact, then compiles it to main.pdf when LaTeX/PDF tooling is available. Fall back to main.tex only when PDF generation is unavailable, refused, or fails.
---

# Slide Script TeX Generator Skill Spec

## 1) Purpose

Generate a standard final PDF handout from:
- `slides.pdf`
- per-slide script text

Workflow:
1. Normalize script.
2. Generate `main.tex` using selected template.
3. Compile `main.tex` to `main.pdf` using available LaTeX/PDF compiler/plugin.
4. Return `main.pdf` as the primary deliverable.
5. Fall back to `main.tex` only when compilation is unavailable, explicitly refused, or not recoverable.

## 2) Required inputs

- Required: exported slide PDF (default filename `slides.pdf`)
- Optional: script file/text, layout preference, language preference

If user only provides PPTX: ask them to export PDF first.

## 3) Defaults

- Layout: `top-slide-manuscript`
- Build artifact filename: `main.tex`
- Primary deliverable filename: `main.pdf`
- Slide reference: `\newcommand{\SlidePDF}{slides.pdf}`
- Language handling: preserve user language

## 4) Required pre-generation questions

Ask unless already answered:
1. Use default layout `top-slide-manuscript`?
2. How is script split (`---`, headings, or unsplit)?
3. Allow adaptive font sizing (top-slide-manuscript only)?
4. How handle blank/divider pages (keep/skip/manual list)?
5. If compilation tooling is missing, should the user install/enable it or accept TeX fallback?

## 5) Script normalization

Priority:
1. `---`
2. headings (`Slide 1`, `Page 1`, `第1页`, etc.)
3. numbered sections
4. approximate split by slide count

Rules:
- Preserve all text.
- Fewer script sections than slides -> add placeholders.
- More script sections than slides -> append to `Extra Notes`.
- Convert lightweight Markdown to LaTeX.
- Escape LaTeX-sensitive characters in prose.

## 6) Template selection

Template/environment mapping (must match):
- `top-slide-manuscript.tex` -> `SlideManuscriptBlock`
- `left-thumbnail-clean.tex` -> `SlideScriptBlock`
- `compact-review-notes.tex` -> `CompactSlideBlock`

All templates must include:
- `%%SLIDE_BLOCKS%%`
- `\newcommand{\SlidePDF}{slides.pdf}`
- no absolute paths

## 7) LaTeX generation rules

- Reference PDF pages with `\includegraphics[page=N,...]{\SlidePDF}`.
- Generate blocks in page order.
- Keep long script content in environment bodies.
- Keep bilingual content intact.
- Ensure each slide block ends with a new page via template design.

Adaptive sizing (top-slide-manuscript only):
- very short: `large` or `Large`
- normal: `normalsize`
- long: `small`
- never use `LARGE`, `huge`, `Huge`
- if unsure, choose smaller

Thresholds:
- 0–60 Chinese chars or 0–45 English words: `large`
- 61–140 Chinese chars or 46–100 English words: `large` or `normalsize`
- 141–350 Chinese chars or 101–230 English words: `normalsize`
- longer: `small`

## 8) PDF compilation and fallback boundary

Preferred compiler/tooling:
- Codex LaTeX/Tectonic plugin/tooling

Local fallback compiler:
- `xelatex main.tex` twice

Alternative local command:
- `tectonic main.tex`

Fallback to `main.tex` only if:
1. required PDF compilation plugin/tooling is unavailable;
2. user explicitly refuses enabling/installing it;
3. compilation fails after reasonable automatic fixes;
4. environment cannot write or return PDF artifacts;
5. user explicitly asks for TeX source only.

## 9) Post-generation checklist

Follow `references/post-generation-checklist.md`.

## 10) Output format

Default successful output:
- `main.pdf`
- short note that `main.tex` was used as build artifact

Fallback output:
- complete `main.tex` source
- exact compile commands
- short explanation why PDF was not produced
