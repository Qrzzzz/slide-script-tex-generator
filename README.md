<div align="center">

# 📝 Slide Script TeX Generator

### Codex Skill for PDF-first speaker handouts from slide PDFs and scripts

[中文](./README-CN.md) · [Examples](./examples) · [Templates](./assets/templates) · [Skill Spec](./SKILL.md) · [Install](./INSTALL.md)

![Codex Skill](https://img.shields.io/badge/Codex-Skill-111827)
![Primary Output](https://img.shields.io/badge/Primary%20Output-main.pdf-0F766E)
![Build Artifact](https://img.shields.io/badge/Build%20Artifact-main.tex-1D4ED8)
![Input](https://img.shields.io/badge/Input-slides.pdf%20%2B%20script-FF5722)
![License](https://img.shields.io/badge/License-LICENSE-blue)

</div>

## What this project does

Input:
- exported `slides.pdf`
- slide-by-slide script

Normal workflow:
1. Generate `main.tex` from `slides.pdf` and script.
2. Compile `main.tex` into `main.pdf` through configured LaTeX/PDF tooling.
3. Deliver `main.pdf` as the standard final artifact.

Fallback:
- If PDF compilation is unavailable, rejected, or fails after reasonable attempts, deliver `main.tex` with exact compile instructions.

PDF generation is the preferred and standard completion path.
TeX generation is the necessary intermediate step.
TeX-only output is a fallback path.

![Overview](./assets/overview-en.png)

## Quick start

```text
Use the slide-script-tex-generator skill.

I have slides.pdf and a page-by-page script.
Generate a PDF-first handout with default top-slide-manuscript layout.
Return main.pdf if compilation tooling is available.
If not, return full main.tex and exact compile commands.
```

## Inputs and deliverables

**Required input**
- `slides.pdf`

**Recommended input**
- `script.md` / `script.txt` / pasted script text

**Primary deliverable**
- `main.pdf`

**Intermediate build artifact**
- `main.tex`

**Fallback deliverable**
- `main.tex` (with compile commands and fallback reason)

## Standard PDF-first workflow

1. Normalize and align script sections to slide pages.
2. Generate full `main.tex` using selected template.
3. Attempt compilation to `main.pdf` when tooling is available.
4. Return `main.pdf` as primary deliverable.
5. Fall back to `main.tex` only under documented fallback conditions.

## Layouts

### 1) `top-slide-manuscript` (default)
- Large slide preview on top, manuscript below.
- Supports optional adaptive manuscript font sizing.

### 2) `left-thumbnail-clean`
- Slide thumbnail on left, script on right.

### 3) `compact-review-notes`
- Compact printable review layout.

## Script splitting rules

1. Split by `---`
2. Split by headings (`Slide 1`, `Page 1`, `第1页`)
3. Split by numbered sections
4. If unsplit, approximate by slide count

## Generation behavior

- Missing script sections -> generate placeholders.
- Extra script sections -> append under `Extra Notes`.
- Blank/divider pages -> ask user keep/skip/manual selection.
- Escape LaTeX special characters in prose.

## PDF generation and fallback behavior

Preferred compiler/tooling:
- Codex LaTeX/Tectonic plugin/tooling, when available.

Local fallback compiler:
```bash
xelatex main.tex
xelatex main.tex
```

Alternative local command:
```bash
tectonic main.tex
```

Fallback to TeX-only output only if:
1. required PDF compilation plugin/tooling is unavailable;
2. user explicitly refuses enabling/installing it;
3. compilation fails after reasonable fixes;
4. environment cannot write or return PDF artifact;
5. user explicitly asks for TeX source only.

## Installation

Quick install:

```text
$skill-installer install https://github.com/Qrzzzz/slide-script-tex-generator
```

Detailed setup and PDF-tooling notes: [INSTALL.md](./INSTALL.md).

## Project structure

```text
slide-script-tex-generator/
├── SKILL.md
├── README.md
├── README-CN.md
├── INSTALL.md
├── CONTRIBUTING.md
├── CHANGELOG.md
├── LICENSE
├── assets/
│   ├── overview-en.png
│   ├── overview-cn.png
│   └── templates/
│       ├── top-slide-manuscript.tex
│       ├── left-thumbnail-clean.tex
│       └── compact-review-notes.tex
├── references/
│   ├── template-style-guide.md
│   ├── script-splitting-rules.md
│   ├── latex-compatibility-notes.md
│   └── post-generation-checklist.md
└── examples/
    ├── sample-script.md
    ├── sample-script-bilingual.md
    ├── sample-main-top-slide-manuscript.tex
    ├── sample-main-left-thumbnail.tex
    ├── sample-main-compact-review-notes.tex
    ├── sample-output-extra-notes.tex
    └── sample-output-missing-script.tex
```

## What this skill does not do

- Does not convert PPTX to PDF
- Does not perform OCR
- Does not claim PDF success unless `main.pdf` exists

## Examples

Examples provide reproducible TeX sources. The standard skill workflow compiles them to PDF when compilation tooling is available.

## License

Licensed under [LICENSE](./LICENSE).
