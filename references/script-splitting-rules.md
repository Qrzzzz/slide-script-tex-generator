# Script Splitting Rules

Normalize the user script into slide sections.

## Priority

1. Split by explicit Markdown separators:

```markdown
---
```

2. Split by slide headings:

```text
Slide 1
Page 1
第1页
第 1 页
P1
```

3. Split by obvious numbered sections:

```text
1.
2.
3.
```

4. If no separators exist, split approximately by slide count if the number of PDF pages is known.

## Alignment rule

If the script has explicit separators such as `---`, preserve the order strictly.

The first script section corresponds to PDF page 1.

The second script section corresponds to PDF page 2.

Continue one-to-one alignment unless the user explicitly asks for a different mapping.

If automatic splitting is used, mention in the final response that the alignment is approximate and should be reviewed.

## Preservation rule

Never delete user content.

If there are more script sections than slide pages, keep the remaining text under Extra Notes.

If there are fewer script sections than slide pages, insert empty placeholders.

## Speaker labels

Preserve labels such as:

```text
Qirui:
Deyi:
Speaker A:
Speaker B:
中文提示：
English:
```

Use bold formatting for labels in LaTeX when possible.

## Bilingual content

Preserve both languages.

Do not rewrite the script unless the user explicitly asks for rewriting or polishing.
