# Markdown tables

A markdown table is line art in its source and a rendered table in its output.
The alignment rules here apply to the source only. No renderer cares how the
pipes line up, but every human and every agent that opens the file does, and a
ragged table is unreadable in a diff.

## Choosing this over a drawn table

Use a markdown table wherever markdown is rendered: documentation, README files,
issues, pull requests, agent-facing docs. The renderer draws the rules, so it
adapts to the viewer's width and font.

Use a box-drawn table (see [layout.md](layout.md)) for terminal output, code
comments, log formatting, or anything pasted into a fixed-width context where no
renderer exists.

Never nest one inside the other. Box characters inside a markdown table cell
survive, but the cell widths are then set by the renderer and the drawn figure
will not align with anything.

## Alignment rule

Pad every cell to the width of the widest cell in its column, and make the
separator row match that width. Compute the width in display columns, exactly as
for a drawn figure.

```
| Package  | Files | Size     |
|----------|------:|:--------:|
| core     |    12 |  4.2 KiB |
| renderer |     8 | 11.0 KiB |
```

Each cell is one space, the content padded to the column width, one space. The
separator cell is that same total width filled with dashes, with any alignment
colons occupying the first and last positions of the fill.

What ragged source looks like, same table:

```
| Package | Files | Size |
|---|---:|:---:|
| core | 12 | 4.2 KiB |
| renderer | 8 | 11.0 KiB |
```

Both render identically. The second is what most tools emit and what a hand edit
degrades into after two changes.

## Syntax that matters

- The header row and the separator row are both required. There is no headerless
  table in GitHub Flavored Markdown.
- The separator row needs at least one dash per column. Three is the convention,
  but the alignment rule overrides it: match the column width.
- Alignment markers are colons on the separator row: `:---` left, `---:` right,
  `:---:` centered, bare dashes for the renderer's default. They stay part of the
  cell when padding, so `|:--------:|` is a ten-column cell.
- Alignment markers control the rendered output. Padding inside a source cell
  controls nothing, so pad however keeps the source readable, usually left for
  text and right for numbers so the source previews the rendering.
- Leading and trailing pipes are optional in the specification. Always write
  them; without them the first and last columns have no edge to align to.
- A row with too few cells is padded with empty ones, and a row with too many has
  the extras silently dropped. Cell count must match the header row exactly, and
  a miscount fails quietly rather than erroring.

## Content that will not fit

- A literal `|` inside a cell ends the cell. Escape it as `\|`, including inside
  inline code spans, where escaping still applies.
- Cells cannot contain a newline. Use `<br>` for a line break inside a cell where
  the renderer allows HTML, or restructure the table.
- Block content (lists, code fences, headings) does not work in a cell. Inline
  markup does: emphasis, links, inline code.
- Long cells force horizontal scroll or wrapping depending on the renderer.
  Neither is controllable from markdown, so keep cells short and move detail to
  prose below the table.

## Width traps

Column width is display width, not character count, so everything in
[gotchas.md](gotchas.md) applies to source alignment here.

- CJK content makes a cell twice as wide as its character count suggests. A table
  whose cells look padded to equal length in an editor that counts characters is
  misaligned in the source.
- Emoji width is unpredictable per environment. A table containing emoji cannot
  be reliably source-aligned; align it for the common case and accept the drift.
- Combining marks add characters and no width. Padding by character count
  overpads those cells.

## Editing an existing table

Adding a row wider than the current column widens every row in that column. Do
the whole column, not the one row, or the table degrades one edit at a time.

Never realign a table by eye. Take the widest cell per column, then pad every
cell in that column to it, separator row included.
