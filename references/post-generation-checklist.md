# PDF-first Post-generation Checklist

Before final response:

1. `main.tex` is generated completely.
2. Slide/script alignment is verified.
3. No absolute paths in resource references.
4. LaTeX special characters are escaped in prose.
5. Compilation is attempted when compatible tooling is available.
6. If compilation succeeds, return `main.pdf`.
7. If compilation fails, record error summary and try reasonable fixes.
8. If fallback is necessary, return `main.tex` and exact compile instructions.
9. Do not claim PDF was generated unless `main.pdf` actually exists.

Fallback to `main.tex` only if:
1. required PDF compilation plugin/tooling is unavailable;
2. user explicitly refuses enabling/installing it;
3. compilation fails after reasonable automatic fixes;
4. environment cannot write or return PDF artifacts;
5. user explicitly asks for TeX source only.
