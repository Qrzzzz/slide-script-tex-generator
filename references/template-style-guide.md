# Template Style Guide

## Scope
Rules for `assets/templates/*.tex`.

## Required constants
- Must define `\newcommand{\SlidePDF}{slides.pdf}`.
- Must contain placeholder `%%SLIDE_BLOCKS%%` exactly once.

## Required environment names
- `left-thumbnail-clean.tex` uses `SlideScriptBlock`
- `top-slide-manuscript.tex` uses `SlideManuscriptBlock`
- `compact-review-notes.tex` uses `CompactSlideBlock`

## Page reference and paths
- Reference slide pages with `\includegraphics[page=..., ...]{\SlidePDF}`.
- Do not use absolute paths.

## Pagination behavior
- Each block environment ends with `\newpage`.
- Generator should rely on template pagination; avoid extra repeated page breaks.

## Language compatibility
- Templates must support English and Chinese text.
- Keep font fallback logic (`fontspec` + fallback families).

## Adaptive sizing policy
- Optional per-slide size argument is supported by `top-slide-manuscript`.
- Other templates do not require adaptive sizing arguments.
