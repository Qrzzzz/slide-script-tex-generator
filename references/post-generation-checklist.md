# Post-generation Checklist

Before returning final `main.tex`, verify:

1. The document is complete and compilable as one `.tex` file.
2. Chosen template matches requested/default layout.
3. `%%SLIDE_BLOCKS%%` has been replaced correctly.
4. Every slide block references the expected PDF page number.
5. Missing script sections are represented with placeholders.
6. Extra script sections are preserved in `Extra Notes`.
7. Blank/divider page handling follows explicit user choice.
8. No absolute paths appear in slide references.
9. Chinese/English text remains readable.
10. If compilation is not requested, output stops at `main.tex`.
