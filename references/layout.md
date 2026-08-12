# Layout and construction

Recipes for the figures worth building, and the arithmetic that keeps them
aligned. Character choice is in
[junctions.md](junctions.md); rendering hazards are in
[gotchas.md](gotchas.md).

## The grid model

Treat the output as a fixed grid of cells, one character per cell, and decide
what occupies each cell before writing any line. Drawing line by line and hoping
the columns match is where most defects come from.

A figure is built in three passes:

1. Measure. Compute the display width of every piece of content.
2. Size. Derive column widths, then row positions.
3. Fill. Emit the border rows and the content rows against those numbers.

Display width is not string length. A CJK ideograph occupies two columns, a
combining mark zero. See [gotchas.md](gotchas.md) for the measurement rules.

## Sizing a box

For content lines `c[0..n]` and horizontal padding `p` (one space by default):

```
inner = max(width(c[i])) + 2p
total = inner + 2          # the two border columns
```

The top border is one corner, `inner` copies of the horizontal, one corner. Each
content row is a vertical, `p` spaces, the content padded to `inner - 2p`, `p`
spaces, a vertical.

```
┌──────────────┐
│ short        │
│ a longer one │
└──────────────┘
```

Alignment inside a cell of width `w` for content of width `c`:

- Left: content, then `w - c` spaces.
- Right: `w - c` spaces, then content.
- Centered: `floor((w - c) / 2)` spaces before, the remainder after. Put the odd
  space on the right so a column of centered labels shares a left edge.

## Titles in the border

A title interrupts the top border. It costs `width(title) + 2` columns of rule,
so the box cannot be narrower than that.

```
┌─ Config ─────────┐
│ retries    3     │
│ timeout    30s   │
└──────────────────┘
```

Start with one horizontal before the title so the corner is not crowded. Keep
the title on the top border; a title on the bottom border reads as a caption and
is usually wrong.

## Tables

Column width is the widest cell in that column plus padding. The separator rows
repeat the horizontal per column and place a junction at each boundary. Which
junction depends on the arms: `┬` at the top edge, `┼` in the body, `┴` at the
bottom edge, `├` and `┤` at the left and right edges.

```
┌───────────┬───────┬──────────┐
│ Package   │ Files │     Size │
├───────────┼───────┼──────────┤
│ core      │    12 │  4.2 KiB │
│ renderer  │     8 │ 11.0 KiB │
└───────────┴───────┴──────────┘
```

Align text left and numbers right. Right-aligned numbers let a reader compare
magnitudes down the column, which is the main reason to put numbers in a table.

For a header that carries weight, run the header rows heavy and the body light,
joined by the transition junctions:

```
┏━━━━━━━━━━━┳━━━━━━━┓
┃ Package   ┃ Files ┃
┡━━━━━━━━━━━╇━━━━━━━┩
│ core      │    12 │
│ renderer  │     8 │
└───────────┴───────┘
```

Omit interior verticals for a lighter table when the columns are already
distinct. Keep the outer frame or drop it entirely; a frame missing one side
looks like a rendering failure.

## Spanning cells

A spanning cell changes which arms exist at each boundary, so the separator row
changes character. Where a divider begins below the rule, use `┬`. Where one ends
above it, use `┴`. Where one passes through, use `┼`.

```
┌───────────────────────┐
│ Summary               │
├───────────┬───────────┤
│ Left      │ Right     │
└───────────┴───────────┘
```

The rule generalizes: at every cell of a separator row, ask which of up, down,
left, right actually carry a line, then look the answer up. Never copy a
separator row from another table and adjust it by eye.

## Trees

Four-column prefixes, one per depth level. Each level contributes either a
connector or the whitespace that continues its parent's line.

| Position                 | Prefix |
|--------------------------|--------|
| Item with siblings below | `├── ` |
| Last item at its level   | `└── ` |
| Ancestor line continues  | `│   ` |
| Ancestor line ended      | `    ` |

```
boxmaster
├── CLAUDE.md
├── SKILL.md
└── references
    ├── charset.md
    ├── gotchas.md
    └── junctions.md
```

The rule that matters: a `│` continuation appears at depth `d` only when the
ancestor at depth `d` still has siblings to come. Getting this wrong produces
dangling verticals under a last child, the most common tree defect.

## Flow diagrams

Boxes joined by connectors. Give every box on a row the same height, and attach
connectors at box edges using the junction that matches the arms.

```
┌────────┐      ┌─────────┐      ┌────────┐
│ Source │ ───▸ │ Parser  │ ───▸ │ Render │
└────────┘      └─────────┘      └────────┘
```

A vertical connector leaves a box through its bottom border, which turns that
cell into a `┬`, and arrives at an arrowhead:

```
┌─────────┐
│ Client  │
└────┬────┘
     │
     ▾
┌─────────┐
│ Server  │
└─────────┘
```

The connector column must be identical on every row it crosses. Compute it once
from the box's left edge and reuse it.

For a branch, turn the corner with an arc or a right angle and join the trunk
with a T:

```
    ┌─────────┐
    │ Router  │
    └────┬────┘
         │
    ┌────┴────┐
    │         │
    ▾         ▾
┌───────┐ ┌───────┐
│ Left  │ │ Right │
└───────┘ └───────┘
```

Give a box an odd total width when a connector has to leave its center. An even
width has no center column, so the connector sits half a cell off and the figure
looks skewed.

Prefer the small arrowheads `▴ ▸ ▾ ◂`. The large forms have emoji presentations
and can render double-width.

## Bars and gauges

The eighth blocks give sub-cell resolution. For a value `v` in `[0, 1]` across
`n` cells, the filled length is `v * n`; emit `floor(v * n)` full blocks, then one
partial block chosen from `▏▎▍▌▋▊▉` by the fractional eighth, then spaces.

```
CPU  ████████████▍         62%
Mem  ██████▎               31%
Disk ██████████████████▊   94%
```

Twenty cells here: 62% is 12.4 cells, so twelve full blocks and three eighths.
Pad the unfilled remainder with spaces so the value column stays aligned.

For vertical bars and sparklines use the lower blocks `▁▂▃▄▅▆▇█`, which grow
upward from U+2581. The two ladders run in opposite directions in the code chart;
see [charset.md](charset.md).

## Shading and shadows

`░▒▓` are three fixed densities, not a gradient with even steps. Use them for
distinct states (empty, partial, full), not for smooth ramps.

A drop shadow offsets one column right and one row down. It leaves the top row
shorter than the rest, which is expected for this figure and not a width error:

```
┌──────────┐
│  Dialog  │▒
└──────────┘▒
 ▒▒▒▒▒▒▒▒▒▒▒▒
```

## Nesting

An inner box needs `2 + 2p` columns beyond its content, and the outer box needs
the same beyond the inner box. Nesting more than two deep usually means the
figure wants to be two figures.

```
┌────────────────────────┐
│ ┌────────┐  ┌────────┐ │
│ │ Left   │  │ Right  │ │
│ └────────┘  └────────┘ │
└────────────────────────┘
```

Interior boxes stay lighter than or equal to their container in weight. A heavy
box inside a light one reads as the container.

## Choosing a style

| Style         | Use for                                              |
|---------------|------------------------------------------------------|
| Light `┌─┐`   | the default, everything without a reason to differ   |
| Heavy `┏━┓`   | one emphasized element, a focused pane, a header row |
| Rounded `╭─╮` | soft framing in documentation and CLI output         |
| Double `╔═╗`  | legacy or DOS-styled interfaces                      |
| Dashed `┌┄┐`  | provisional, optional, or inactive elements          |

Use at most two styles in one figure, and let the second one mean something.
