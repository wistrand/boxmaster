# Gotchas

Why a figure that looks correct locally breaks somewhere else. Skim before
shipping art into a terminal, a web page, or a PDF.

## Width

**Box-drawing and block-element characters are East Asian Ambiguous width.**
`EastAsianWidth.txt` classifies U+2500–U+257F and U+2580–U+259F as `A`. A
terminal set to render ambiguous characters as wide, common in CJK locales and a
configurable option in most terminal emulators, draws every one of them two
columns wide. The figure does not merely look different, it breaks: content mixes
narrow ASCII with double-width borders. There is no fix in the art itself. Know
that the target may do this, and prefer figures that survive it (uniform border
characters, no ASCII interleaved with box characters inside a run).

**Display width is not string length.** Count columns the way a terminal does:
most characters are one column, East Asian Wide and Fullwidth are two, combining
marks are zero, and a variation selector is zero. `len()` in most languages and
`wc -c` both give the wrong answer for anything outside ASCII.

**Emoji width is not reliably predictable.** A ZWJ sequence may render as one
double-width glyph, as several glyphs, or as a fallback box, and it differs per
terminal. Never put emoji inside a table cell that must align. If content might
contain emoji, budget for the widest case or truncate.

**Tabs destroy alignment.** A tab expands against the receiving renderer's tab
stops, not yours. Never emit a tab inside a figure, and expand tabs in content
before measuring it.

**Trailing whitespace is not reliably preserved.** Editors, linters, and CI
checks strip it; some paste paths add it. Any figure that depends on trailing
spaces (right-padded content past the final border, a drop shadow) can arrive
damaged. Prefer figures whose last character on each line is a border character.

## Fonts and rendering

**Not every monospace font covers the whole block.** The light and double sets
are near-universal. The mixed light/heavy junctions (U+251D–U+254B), the
quadrant blocks (U+2596–U+259F), and the arcs are the usual gaps. A missing glyph
falls back to another font, which is usually a different width and always a
different weight, so one character silently shifts the rest of the line. Test on
the target font before relying on the mixed-weight junctions.

**Terminals may draw box characters themselves.** VTE-based terminals and
several others substitute internally-generated line art for the box-drawing
range, ignoring the font. This is normally an improvement, since the lines
connect exactly, but it means what is seen locally may not reflect the font at
all, and a font test that passes in one terminal proves nothing about another.

**In HTML, line-height and letter-spacing break vertical runs.** A `│` column
shows gaps unless `line-height: 1` and `letter-spacing: 0` are set on a monospace
element. Set `font-variant-ligatures: none` too; some coding fonts ligate
sequences of box characters.

**Outside a code block, markdown renders in a proportional font** and the figure
collapses. Always fence box art. The same applies to any pipeline that reflows
text: chat clients, issue trackers, email in HTML mode.

**A figure wider than the viewport wraps and is destroyed.** Wrapping inserts a
break mid-row, so every row below it is offset. Keep figures at or under 80
columns when the target width is unknown.

## Characters that look right and are not

**`─` is not `-`, `–`, or `—`.** U+2500 is the box-drawing horizontal; hyphen,
en dash, and em dash are punctuation with different widths and no line-joining
behavior. Editors and word processors with smart-punctuation substitution will
convert typed hyphens into dashes inside a figure. Disable it, or paste rather
than type.

**`│` is not `|`.** U+2502 versus ASCII vertical line. The ASCII form does not
connect to anything and sits at a different height.

**`▶` and `▼` have emoji presentation sequences.** Followed by U+FE0F, or in
environments that default these to emoji, they render as colored double-width
glyphs. Use the small forms `▸ ▾ ◂ ▴` instead, which have no emoji
presentation.

**Heavy and double look similar at small sizes and behave completely
differently.** Heavy is a weight and combines with light at every junction.
Double is a style and combines with light only per-axis, never with heavy. If a
figure needs a junction that does not exist, this confusion is usually the cause.

## Content inside boxes

**Right-to-left text reorders visually inside a cell.** The bidirectional
algorithm can pull neighboring punctuation and the border character itself into
the reordered run. Isolate RTL content with U+2068 and U+2069 (first strong
isolate and pop directional isolate), or keep RTL out of framed layouts.

**Box art is noise to a screen reader.** Every border character is announced. A
figure carrying real information needs a text alternative, and a decorative one
should be marked as decorative where the format allows it.

**`cut`, `awk`, and friends count bytes by default.** `cut -c` on a UTF-8 figure
splits multi-byte sequences. Use `cut` with character semantics only where the
locale supports it, `wc -m` rather than `wc -c`, and prefer a real Unicode-aware
tool for anything measuring width.

## Windows

**The console needs a UTF-8 code page.** `chcp 65001` before output, and a
TrueType console font. The legacy raster fonts carry only the CP437 subset.

**CP437 has no heavy and no rounded characters.** The DOS code page provided the
light and double sets only, which is why legacy interfaces use double lines for
emphasis where a modern one would use heavy. Where CP437 compatibility matters,
restrict output to the light and double sets.

## Verifying a figure

Measure before shipping, and measure in display columns, not characters.

For pure-ASCII content, character count equals display width, so counting
characters per row is enough: every row of a box must produce the same number.

For anything else, count by the rules at the top of this file: East Asian Wide
and Fullwidth characters are two columns, combining marks and variation
selectors are zero, everything else is one. A row of CJK content whose character
count matches its neighbours is a row that is too wide.

Two checks that need no tooling:

- Put the cursor on a vertical border character and move it down the figure. Any
  editor's column indicator shows the drift immediately, and drift is the defect.
- Read each junction against its four neighbours and confirm the arms match what
  is actually drawn there. Most broken figures have a correct-looking junction
  with a wrong arm.

When a figure is large or its content is not ASCII, write a throwaway width
script outside this skill and run it. Nothing here ships as executable code, and
nothing here should be pasted and run without reading it first.
