---
name: boxmaster
description: Draw boxes, tables, trees, and diagrams with Unicode box-drawing characters (U+2500-257F) and block elements. Covers the full glyph inventory, junction and weight-transition rules, sizing and alignment math, and the font, terminal, and character-width traps that break line art. Also covers markdown table syntax and keeping markdown table source aligned. Use when producing or repairing framed text, markdown tables, ASCII or Unicode tables, tree listings, flow and architecture diagrams, terminal UI mockups, progress bars, or sparklines, and whenever a figure's lines fail to connect or a table's columns fail to line up.
---

# Box drawing

Produce line art that is correct, not merely plausible. A figure is correct when
its columns line up, every junction matches the arms that actually touch it, and
it survives the font and terminal it lands in.

## Rules

Never break these. Each one produces a visibly broken figure.

- Every row of a box or table has the same display width, and a vertical line
  occupies the same column on every row it crosses. Ragged right edges are fine
  for trees and connector diagrams; misaligned columns never are.
- Count display columns, not characters and not bytes. East Asian Wide and
  Fullwidth characters take two columns, combining marks and variation selectors
  take zero.
- One style family per frame. Never put a double corner on a light frame or a
  rounded corner on a heavy one.
- Heavy and double never meet at a junction. Unicode has no such glyph, so there
  is no correct character to write. Change one of the two lines instead.
- A weight change along a line uses a transition glyph (`╼ ╾ ╽ ╿`) or a mixed
  junction. Never butt `─` directly against `━`.
- Rounded corners are light only. There are no heavy or double arcs, and no
  rounded T-junctions, so a rounded frame still uses `├ ┬ ┤ ┴ ┼` inside.
- Dashed characters have no corners and no junctions. Draw the frame solid in the
  matching weight and dash only the straight runs.
- Always fence box art in markdown, and use a monospace element in HTML.
- Never leave trailing whitespace after the last border character of a row.
- Never substitute ASCII (`+ - |`) unless the user asked for an ASCII-only figure.

## Method

1. Measure. Compute the display width of every piece of content first.
2. Size. Derive column widths, then the row and connector positions.
3. Fill. Emit border rows and content rows against those numbers.

Decide what occupies each cell before writing any line. Drawing line by line and
hoping the columns match is where most defects come from.

Before returning a figure, check all five:

1. Box rows equal in width, vertical lines column-aligned.
2. Every corner and junction matches the arms that actually touch it.
3. Frame closed: four corners, no line running past its corner.
4. Padding consistent, one space each side by default.
5. Content that could be wide (CJK, emoji) measured as wide.

## Large figures

Above roughly twenty rows, or for any figure that overlays elements on a fill, or
any figure whose content is not ASCII, stop typing rows and generate them.

Build a placement table: for every row, the column and the text of each fragment
that lands in it. Paint those fragments into a fixed-width buffer one row at a
time, pad each row to the figure's single width, and confirm every row matches
that width before showing the figure to anyone. Revising then means changing
numbers in the table rather than redrawing every row, which is what makes a
second pass cheap.

- Always compute column positions rather than counting them. One miscounted
  leading space shifts a label or a border, and nothing below it lines up.
- Always keep the generator outside this skill. It is scratch work for one
  figure, never part of what ships, and never written into a user's project
  unless they ask for it.
- Never treat a passing width check as a correct figure. It proves the rows are
  the same length. It says nothing about an element that is missing, drawn twice,
  or in the wrong place, so read the figure too, and never report a figure as
  verified for a property that was not the risk.

## Quick reference

Enough for most figures. Reach for the reference files when the figure needs a
mixed-weight junction or the target environment is unusual.

| Style   | Corners   | T-junctions | Cross | Lines |
|---------|-----------|-------------|-------|-------|
| Light   | `┌ ┐ └ ┘` | `├ ┬ ┤ ┴`   | `┼`   | `─ │` |
| Heavy   | `┏ ┓ ┗ ┛` | `┣ ┳ ┫ ┻`   | `╋`   | `━ ┃` |
| Double  | `╔ ╗ ╚ ╝` | `╠ ╦ ╣ ╩`   | `╬`   | `═ ║` |
| Rounded | `╭ ╮ ╰ ╯` | none        | none  | `─ │` |

Light is the default. It has the widest font coverage and no junction gaps. Use
heavy for one emphasized edge, double only for legacy or DOS-styled output, and
rounded for a softer frame in documentation and CLI output. Use at most two
styles in one figure, and let the second one mean something.

```
┌──────────────┐    ╭──────────────╮    ┏━━━━━━━━━━━━━━┓
│ light        │    │ rounded      │    ┃ heavy        ┃
└──────────────┘    ╰──────────────╯    ┗━━━━━━━━━━━━━━┛
```

Sizing a box: `inner = max(content width) + 2 × padding`, and the total width is
`inner + 2` for the border columns. Give a box an odd total width when a
connector leaves its center, because an even width has no center column.

## Markdown tables

A markdown table is line art in its source, so the same discipline applies there:
pad every cell to its column width and match the separator row to that width.

```
| Package  | Files | Size     |
|----------|------:|:--------:|
| core     |    12 |  4.2 KiB |
| renderer |     8 | 11.0 KiB |
```

Source alignment changes nothing in the rendered output. It is for whoever opens
the file next, and for the diff. Always align it anyway; never realign by eye.

Use a markdown table wherever markdown is rendered, and a drawn table for
terminal output and other fixed-width contexts with no renderer.

## References

Read the one the task needs. Each stands alone.

- [references/junctions.md](references/junctions.md): arms in, character out. Every light/heavy combination, the single/double set, weight transitions, and the combinations Unicode does not have. This is the file to open mid-draw.
- [references/layout.md](references/layout.md): construction recipes and the arithmetic. Boxes, titles in borders, tables, spanning cells, trees, flow diagrams, bars, shading, nesting.
- [references/charset.md](references/charset.md): the complete inventory with codepoints, plus how to read a Unicode name as a specification of the arms it draws.
- [references/markdown-tables.md](references/markdown-tables.md): markdown table syntax and source alignment. Separator rows, alignment colons, escaping, cell-count failures, and what breaks a cell.
- [references/gotchas.md](references/gotchas.md): why a correct figure renders wrong elsewhere. Ambiguous width, font coverage, HTML line-height, emoji presentation, Windows and CP437. Read before shipping art into an unknown environment.
