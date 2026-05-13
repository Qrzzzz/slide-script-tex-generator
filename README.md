<div align="center">

# 📝 Slide Script TeX Generator

### Codex Skill: generate editable LaTeX speaker notes from slide PDFs and scripts

[中文](./README-CN.md) · [Examples](./examples) · [Templates](./assets/templates) · [Skill Spec](./SKILL.md) · [Install](./INSTALL.md)

![Codex Skill](https://img.shields.io/badge/Codex-Skill-111827)
![Core Output](https://img.shields.io/badge/Core%20Output-main.tex-0F766E)
![Input](https://img.shields.io/badge/Input-slides.pdf%20%2B%20script-FF5722)
![License](https://img.shields.io/badge/License-LICENSE-blue)

</div>

## What this project does

This repository provides a Codex Skill for one job:

- **Input**: exported `slides.pdf` + slide-by-slide speech script
- **Core output**: one complete, editable, compilable `main.tex`
- **Optional output**: `main.pdf`, only if the user/environment compiles later

The skill does not treat PDF compilation as its core responsibility.

![Overview](./assets/overview-en.png)

## Quick start

Use the skill with default settings (`top-slide-manuscript`):

```text
Use the slide-script-tex-generator skill.

I have slides.pdf and a page-by-page script.
Generate main.tex with the default top-slide-manuscript layout.
Only output the full main.tex source.
```

## Inputs and outputs

**Required input**
- `slides.pdf` (already exported from PPT/Keynote/Google Slides)

**Recommended input**
- `script.md` / `script.txt` / pasted script text

**Core output**
- `main.tex`

**Optional output (outside core skill output)**
- `main.pdf` after manual or tool-based compilation

## Layouts

### 1) `top-slide-manuscript` (default)
- Large slide preview on top, manuscript below.
- Best for rehearsal and full speaking scripts.
- Supports optional per-slide adaptive manuscript font sizing.

### 2) `left-thumbnail-clean`
- Slide thumbnail on the left, script on the right.
- Best for structured handouts and bilingual use.

### 3) `compact-review-notes`
- Dense review format with smaller visual footprint.
- Best for quick review and print efficiency.

## Script splitting rules

Script normalization priority:
1. Split by `---`
2. Split by headings like `Slide 1`, `Page 1`, `第1页`
3. Split by numbered sections
4. If no delimiter exists, split approximately by slide count

## Generation behavior

- **Missing script sections**: generate empty placeholders for unmatched slides.
- **Extra script sections**: keep all content and append extras under an `Extra Notes` section.
- **Blank/divider pages**: do not silently guess; ask user whether to keep or skip.
- **Adaptive font sizing**: enabled only for `top-slide-manuscript` when user allows it.

## Optional compilation

Compilation is outside the core skill output boundary. The skill’s normal completion is `main.tex`.

- **Default local compile command**:
  ```bash
  xelatex main.tex
  xelatex main.tex
  ```
- **Optional Tectonic command**:
  ```bash
  tectonic main.tex
  ```

## Installation in Codex

Quick install:

```text
$skill-installer install https://github.com/Qrzzzz/slide-script-tex-generator
```

For manual install commands and troubleshooting, see [INSTALL.md](./INSTALL.md).

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
    ├── sample-output-top-slide-manuscript.tex
    ├── sample-output-left-thumbnail.tex
    ├── sample-output-compact-review-notes.tex
    ├── sample-output-extra-notes.tex
    └── sample-output-missing-script.tex
```

## What this skill does not do

- Does not convert PPTX to PDF
- Does not perform OCR
- Does not require compilation to finish core task
- Does not use absolute local paths in templates

## Examples

See [examples](./examples) for scripts and generated `.tex` outputs across all layouts and mismatch cases.

## License

Licensed under [LICENSE](./LICENSE).
