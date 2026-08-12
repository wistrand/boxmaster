# Character reference

The complete inventory. For choosing a character at an intersection, use
[junctions.md](junctions.md) instead; this file answers what
exists.

Two blocks carry almost everything: Box Drawing (U+2500–U+257F, 128 characters,
all assigned) and Block Elements (U+2580–U+259F, 32 characters). Both are stable
since Unicode 1.0 and present in every serious monospace font.

## Reading the Unicode names

The names are a specification, not a label. Each one lists the arms that leave
the glyph and the weight of each, so a name identifies exactly one character and
can be read back into a picture.

- `BOX DRAWINGS LIGHT DOWN AND RIGHT` (`┌`): arms down and right, both light.
- `BOX DRAWINGS DOWN HEAVY AND RIGHT LIGHT` (`┎`): arm down is heavy, arm right
  is light.
- `BOX DRAWINGS UP HEAVY AND RIGHT DOWN LIGHT` (`┞`): up heavy, right and down
  light. Three arms, so it is a T-junction opening right.

A weight word applies to every arm named before it in the same clause. Arms not
named are absent. `HORIZONTAL` means left and right together, `VERTICAL` means up
and down together.

## Light and heavy straight lines

| Glyph | Code   | Name             |
|-------|--------|------------------|
| `─`   | U+2500 | LIGHT HORIZONTAL |
| `━`   | U+2501 | HEAVY HORIZONTAL |
| `│`   | U+2502 | LIGHT VERTICAL   |
| `┃`   | U+2503 | HEAVY VERTICAL   |

## Dashed lines

Three dash densities, each in light and heavy, horizontal and vertical. Density
rises with the dash count: triple dash is the coarsest, quadruple finer, double
dash coarsest of all in practice because the segments are longest.

| Dashes    | Light H | Heavy H | Light V | Heavy V | Codes         |
|-----------|---------|---------|---------|---------|---------------|
| Double    | `╌`     | `╍`     | `╎`     | `╏`     | U+254C–U+254F |
| Triple    | `┄`     | `┅`     | `┆`     | `┇`     | U+2504–U+2507 |
| Quadruple | `┈`     | `┉`     | `┊`     | `┋`     | U+2508–U+250B |

There are no dashed corners and no dashed junctions. A dashed frame uses solid
corners of the matching weight. Fonts vary in where they place the gaps, so a
dashed vertical rarely aligns visually with a dashed horizontal; that is
expected, not a defect.

## Light and heavy corners

Ordered by which arms are heavy. Each group of four covers: neither, horizontal
arm heavy, vertical arm heavy, both.

| Corner       | Both light | H heavy | V heavy | Both heavy | Codes         |
|--------------|------------|---------|---------|------------|---------------|
| Down + right | `┌`        | `┍`     | `┎`     | `┏`        | U+250C–U+250F |
| Down + left  | `┐`        | `┑`     | `┒`     | `┓`        | U+2510–U+2513 |
| Up + right   | `└`        | `┕`     | `┖`     | `┗`        | U+2514–U+2517 |
| Up + left    | `┘`        | `┙`     | `┚`     | `┛`        | U+2518–U+251B |

## Light and heavy junctions

Every combination of light and heavy arms exists: 8 per T-junction family (three
arms), 16 for the cross. See [junctions.md](junctions.md) for
the full arms-to-glyph tables.

| Family                | All light | All heavy | Range         | Count |
|-----------------------|-----------|-----------|---------------|-------|
| T opening right (`├`) | `├`       | `┣`       | U+251C–U+2523 | 8     |
| T opening left (`┤`)  | `┤`       | `┫`       | U+2524–U+252B | 8     |
| T opening down (`┬`)  | `┬`       | `┳`       | U+252C–U+2533 | 8     |
| T opening up (`┴`)    | `┴`       | `┻`       | U+2534–U+253B | 8     |
| Cross (`┼`)           | `┼`       | `╋`       | U+253C–U+254B | 16    |

## Double lines

Double is a separate style, not a third weight. It joins only with light, never
with heavy.

| Glyph | Code   | Name              |
|-------|--------|-------------------|
| `═`   | U+2550 | DOUBLE HORIZONTAL |
| `║`   | U+2551 | DOUBLE VERTICAL   |

Corners and junctions come in three variants each: horizontal double only,
vertical double only, both double. The all-single variant is the light character
from the tables above.

| Position     | H double | V double | Both | Codes                  |
|--------------|----------|----------|------|------------------------|
| Down + right | `╒`      | `╓`      | `╔`  | U+2552, U+2553, U+2554 |
| Down + left  | `╕`      | `╖`      | `╗`  | U+2555, U+2556, U+2557 |
| Up + right   | `╘`      | `╙`      | `╚`  | U+2558, U+2559, U+255A |
| Up + left    | `╛`      | `╜`      | `╝`  | U+255B, U+255C, U+255D |
| T right      | `╞`      | `╟`      | `╠`  | U+255E, U+255F, U+2560 |
| T left       | `╡`      | `╢`      | `╣`  | U+2561, U+2562, U+2563 |
| T down       | `╤`      | `╥`      | `╦`  | U+2564, U+2565, U+2566 |
| T up         | `╧`      | `╨`      | `╩`  | U+2567, U+2568, U+2569 |
| Cross        | `╪`      | `╫`      | `╬`  | U+256A, U+256B, U+256C |

The style applies per axis, never per arm. There is no character with a double
left arm and a single right arm on the same horizontal.

## Rounded corners

| Glyph | Code   | Name                     | Replaces |
|-------|--------|--------------------------|----------|
| `╭`   | U+256D | LIGHT ARC DOWN AND RIGHT | `┌`      |
| `╮`   | U+256E | LIGHT ARC DOWN AND LEFT  | `┐`      |
| `╯`   | U+256F | LIGHT ARC UP AND LEFT    | `┘`      |
| `╰`   | U+2570 | LIGHT ARC UP AND RIGHT   | `└`      |

Light only. There is no heavy or double arc, and no rounded T-junction, so a
rounded frame still uses square `├ ┬ ┤ ┴ ┼` for interior dividers.

## Diagonals

| Glyph | Code   | Name                                     |
|-------|--------|------------------------------------------|
| `╱`   | U+2571 | LIGHT DIAGONAL UPPER RIGHT TO LOWER LEFT |
| `╲`   | U+2572 | LIGHT DIAGONAL UPPER LEFT TO LOWER RIGHT |
| `╳`   | U+2573 | LIGHT DIAGONAL CROSS                     |

They do not join with the orthogonal lines. A diagonal meeting a frame leaves a
visible gap in most fonts.

## Half lines and stubs

One arm only. Used for line ends, arrow tails, and single-cell ticks.

| Direction | Light | Code   | Heavy | Code   |
|-----------|-------|--------|-------|--------|
| Left      | `╴`   | U+2574 | `╸`   | U+2578 |
| Up        | `╵`   | U+2575 | `╹`   | U+2579 |
| Right     | `╶`   | U+2576 | `╺`   | U+257A |
| Down      | `╷`   | U+2577 | `╻`   | U+257B |

## Weight transitions

Two arms, one light and one heavy, in line with each other. These are the only
correct way to change weight mid-run.

| Glyph | Code   | Name                       | Reads as      |
|-------|--------|----------------------------|---------------|
| `╼`   | U+257C | LIGHT LEFT AND HEAVY RIGHT | `─╼━`         |
| `╽`   | U+257D | LIGHT UP AND HEAVY DOWN    | `│` above `┃` |
| `╾`   | U+257E | HEAVY LEFT AND LIGHT RIGHT | `━╾─`         |
| `╿`   | U+257F | HEAVY UP AND LIGHT DOWN    | `┃` above `│` |

There is no light-to-double transition character.

## Block elements

U+2580–U+259F. Used for shading, fills, bars, and shadows rather than frames.

| Glyph        | Code          | Name                                         |
|--------------|---------------|----------------------------------------------|
| `▀`          | U+2580        | UPPER HALF BLOCK                             |
| `▁`–`▇`      | U+2581–U+2587 | LOWER ONE EIGHTH through SEVEN EIGHTHS BLOCK |
| `█`          | U+2588        | FULL BLOCK                                   |
| `▉`–`▏`      | U+2589–U+258F | LEFT SEVEN EIGHTHS through ONE EIGHTH BLOCK  |
| `▐`          | U+2590        | RIGHT HALF BLOCK                             |
| `░`          | U+2591        | LIGHT SHADE                                  |
| `▒`          | U+2592        | MEDIUM SHADE                                 |
| `▓`          | U+2593        | DARK SHADE                                   |
| `▔`          | U+2594        | UPPER ONE EIGHTH BLOCK                       |
| `▕`          | U+2595        | RIGHT ONE EIGHTH BLOCK                       |
| `▖▗▘▙▚▛▜▝▞▟` | U+2596–U+259F | quadrant blocks                              |

The eighth-block ladders `▁▂▃▄▅▆▇█` and `▏▎▍▌▋▊▉█` are the standard sub-cell
resolution trick for bars and sparklines. Vertical and horizontal ladders run in
opposite directions: the lower blocks grow upward from U+2581, the left blocks
grow leftward from U+258F.

## Arrows and markers

Not in either block, but needed for diagrams. All are East Asian Ambiguous
width; see [gotchas.md](gotchas.md).

| Glyph     | Code                           | Use                    |
|-----------|--------------------------------|------------------------|
| `← ↑ → ↓` | U+2190–U+2193                  | line arrowheads        |
| `▴ ▸ ▾ ◂` | U+25B4, U+25B8, U+25BE, U+25C2 | small solid arrowheads |
| `▲ ▶ ▼ ◀` | U+25B2, U+25B6, U+25BC, U+25C0 | large solid arrowheads |
| `● ○ ◆ ◇` | U+25CF, U+25CB, U+25C6, U+25C7 | nodes and bullets      |

Prefer the small forms `▴ ▸ ▾ ◂`. The large ones have emoji presentation
sequences, and some environments render `▶` as a colored, double-width emoji.

## Beyond these blocks

Symbols for Legacy Computing (U+1FB00–U+1FBFF, added in Unicode 13) and its
supplement (U+1CC00–U+1CEBF, added in Unicode 16) contain sextant blocks,
one-eighth scan lines, and additional box-drawing variants. Font coverage is thin
outside a few terminal fonts, and these are outside the Basic Multilingual Plane
in the supplement's case. Verify rendering in the actual target before using
anything from either block; treat both as unavailable by default.
