# Script Splitting Rules

## Priority order
1. Split by `---`
2. Split by headings (`Slide 1`, `Page 1`, `第1页`, `第 1 页`, `P1`)
3. Split by numbered sections
4. If unsplit, approximate split by slide count

## Alignment rules
- Preserve all source text.
- If script sections < slide pages: create empty section placeholders.
- If script sections > slide pages: append overflow under `Extra Notes`.
- Never drop content silently.

## Separator cleanup
- Remove structural separator headings from body text when they carry no speaking content.
- Keep headings if they include meaningful script content.
