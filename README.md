# slide-script-tex-generator

A minimal Codex Skill for generating a standalone LaTeX speaker handout from a slide PDF and a speech script.

## What it does

Given:

- `slides.pdf`
- a speech script in Markdown, txt, or pasted text

It generates:

- one complete `.tex` file

The `.tex` file references individual pages from the slide PDF with:

```latex
\includegraphics[page=1]{slides.pdf}
```

## What it does not do

This skill is intentionally narrow. It does not convert PPTX, compile LaTeX, perform OCR, install dependencies, or create a full project folder.

## Recommended working directory

```text
my-presentation/
  slides.pdf
  script.md
  main.tex
```

## Available styles

### left-thumbnail-clean

Default style. Slide thumbnail on the left, script on the right.

### top-slide-manuscript

Large slide preview on top, manuscript below.

### compact-review-notes

Compact visual layout for printing, rehearsal, and review. It still keeps one slide-script unit per page.

## Generation Checklist

After generating the `.tex`, the skill checks common issues such as:

- slide/script mismatch
- missing `\newpage`
- wrong PDF filename
- raw Markdown left in LaTeX
- unescaped LaTeX special characters
- absolute local paths
- missing placeholders for slides without scripts

## Out of scope

LaTeX compilation is outside this skill. The skill stops after producing one `.tex` source file.
