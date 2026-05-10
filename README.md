<div align="center">

# 📝 Slide Script TeX Generator

### A lightweight Codex Skill for generating LaTeX speaker handouts from slide PDFs

**Slide PDF / Speech Script / LaTeX Source / Speaker Notes / Bilingual Friendly**

[中文](./README-CN.md) · [Examples](./examples) · [Templates](./assets/templates) · [Skill](./SKILL.md)

![Codex Skill](https://img.shields.io/badge/Codex-Skill-111827)
![LaTeX](https://img.shields.io/badge/Output-LaTeX-008080?logo=latex&logoColor=white)
![PDF](https://img.shields.io/badge/Input-PDF-FF5722?logo=adobeacrobatreader&logoColor=white)
![License](https://img.shields.io/github/license/Qrzzzz/slide-script-tex-generator)

</div>

---

## Overview

**Slide Script TeX Generator** is a minimal Codex Skill that turns an exported slide PDF and a speech script into a standalone LaTeX source file.

It is designed for students, presenters, teachers, and anyone who wants to organize presentation scripts together with slide previews.

![Overview](./assets/overview-en.png)

---

## What it does

Given:

```text
slides.pdf
script.md / script.txt / pasted text
```

It generates:

```text
main.tex
```

The generated LaTeX file references individual pages from the slide PDF directly:

```latex
\includegraphics[page=1,width=0.4\textwidth]{slides.pdf}
```

This means the slide PDF stays as the visual source, while the speech script is aligned page by page in a clean LaTeX handout.

---

## Core features

- Generate a standalone `.tex` file from a slide PDF and a speech script
- Align script content with corresponding slide pages
- Support Markdown, plain text, txt, or pasted script content
- Support Chinese, English, and bilingual scripts
- Provide three high-compatibility LaTeX templates
- Keep the workflow simple, editable, and GitHub-friendly

---

## Available layouts

### `left-thumbnail-clean`

Default layout.

The slide thumbnail appears on the left, and the speech script appears on the right.

Best for:

- formal presentations
- speaker handouts
- bilingual scripts
- slide-by-slide explanation

---

### `top-slide-manuscript`

The slide preview appears at the top, and the full manuscript appears below.

Best for:

- rehearsal
- memorization
- full speech scripts
- presentation practice

---

### `compact-review-notes`

A compact layout for printing, review, and rehearsal.

Best for:

- short notes
- print-friendly documents
- quick review
- reduced page usage

---

## Recommended working directory

```text
my-presentation/
  slides.pdf
  script.md
  main.tex
```

The generated `.tex` file assumes that `slides.pdf` is in the same folder.

---

## Example usage with Codex

You can ask Codex:

```text
Use the slide-script-tex-generator skill.

I have slides.pdf and the following speech script.
Generate a left-thumbnail-clean LaTeX handout.
Only output the final main.tex source.
```

Or:

```text
Use the slide-script-tex-generator skill.

Generate a compact review version from slides.pdf and script.md.
The script is separated by ---.
```

---

## Script splitting

The skill tries to split the script in this order:

1. Markdown separators:

```markdown
---
```

2. Slide headings:

```text
Slide 1
Page 1
第1页
第 1 页
P1
```

3. Numbered sections

4. Approximate splitting by slide count, if needed

The skill preserves all user content. Extra script sections are kept as additional notes.

---

## Generation checklist

After generating the `.tex`, the skill checks common issues such as:

- slide/script mismatch
- missing `\newpage`
- wrong PDF filename
- raw Markdown left in LaTeX
- unescaped LaTeX special characters
- absolute local paths
- missing placeholders for slides without scripts

---

## What this skill does not do

This skill intentionally stays narrow.

It does not:

- convert PPTX to PDF
- compile LaTeX
- perform OCR
- install dependencies
- create a full project folder
- rasterize PDF pages into images

Please export your presentation as `slides.pdf` before using this skill.

---

## Project structure

```text
slide-script-tex-generator/
  SKILL.md
  README.md
  README.cn.md
  assets/
    overview-en.png
    overview-cn.png
    templates/
      left-thumbnail-clean.tex
      top-slide-manuscript.tex
      compact-review-notes.tex
  references/
    template-style-guide.md
    script-splitting-rules.md
    latex-compatibility-notes.md
  examples/
    sample-script.md
    sample-output-left-thumbnail.tex
```

---

## Compile the generated file

After Codex generates `main.tex`, place it in the same directory as `slides.pdf`, then compile with XeLaTeX:

```bash
xelatex main.tex
xelatex main.tex
```

LaTeX compilation is outside the skill itself. The skill stops after generating the `.tex` source.

---

## License

This project is released under the MIT License.
