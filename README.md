# boxmaster

An Agent Skill for drawing boxes, tables, trees, and diagrams with Unicode
box-drawing characters.

There is no program here. The repository is the knowledge: the full glyph
inventory of the Unicode box-drawing block, the rules for joining lines of
different weights and styles, the layout math that keeps a figure aligned, and
the rendering traps that make good-looking art fall apart in someone else's
terminal.

It exists because box art is easy to produce and hard to produce correctly. A
figure that looks right in one editor breaks in a CJK locale, in a proportional
font, or in a font missing the heavy junction glyphs. Most of the material here
is about avoiding that.

## Examples

> Draw me a table of the package sizes: core is 12 files and 4.2 KiB, renderer
> is 8 files and 11.0 KiB, cli is 3 files and 1.8 KiB.

Each column comes out as wide as its widest cell plus one space of padding on
each side, and the numeric columns are right aligned.

```
┌──────────┬───────┬──────────┐
│ Package  │ Files │     Size │
├──────────┼───────┼──────────┤
│ core     │    12 │  4.2 KiB │
│ renderer │     8 │ 11.0 KiB │
│ cli      │     3 │  1.8 KiB │
└──────────┴───────┴──────────┘
```

> Diagram SKILL.md branching to junctions, layout and gotchas.

The boxes are equal in width and the junctions match the arms that touch them:
`┬` where the line leaves a box downward, `┴` where it arrives from above, `┼`
where the trunk crosses the bus.

```
              ┌───────────┐
              │ SKILL.md  │
              └─────┬─────┘
                    │
      ┌─────────────┼─────────────┐
      │             │             │
┌─────┴─────┐ ┌─────┴─────┐ ┌─────┴─────┐
│ junctions │ │  layout   │ │  gotchas  │
└───────────┘ └───────────┘ └───────────┘
```

> Put the repo tree and a table of reference file sizes side by side in one
> framed panel.

The header cell spans both columns, so the top border carries no `┬` and the
rule below it does. The inner table is a nesting: light inside light, sized on
its own content before the outer frame is sized around it.

```
┌──────────────────────────────────────────────────────────┐
│ Repository layout                                        │
├────────────────────────────┬─────────────────────────────┤
│ boxmaster                  │ ┌─────────────────┬───────┐ │
│ ├── SKILL.md               │ │ Reference       │ Lines │ │
│ ├── CLAUDE.md              │ ├─────────────────┼───────┤ │
│ └── references             │ │ junctions       │   157 │ │
│     ├── junctions.md       │ │ layout          │   257 │ │
│     ├── layout.md          │ │ charset         │   201 │ │
│     ├── charset.md         │ │ markdown-tables │   100 │ │
│     ├── markdown-tables.md │ │ gotchas         │   133 │ │
│     └── gotchas.md         │ └─────────────────┴───────┘ │
└────────────────────────────┴─────────────────────────────┘
```

## Install

Symlink the repository into your personal skills directory:

```bash
ln -s "$PWD" ~/.claude/skills/boxmaster
```

Use `.claude/skills/boxmaster` inside a project instead to scope it to that
project. Claude loads the skill's name and description at startup and reads the
rest only when a request calls for line art.

## Layout

```
boxmaster
├── SKILL.md                rules, method, quick reference
├── CLAUDE.md               how to work on the skill
└── references
    ├── junctions.md        arms in, character out
    ├── layout.md           construction recipes and sizing math
    ├── charset.md          the complete inventory
    ├── markdown-tables.md  syntax and source alignment
    └── gotchas.md          width, font, and terminal traps
```

## Source of truth

The Unicode Standard, code charts for Box Drawing (U+2500–U+257F) and Block
Elements (U+2580–U+259F): https://www.unicode.org/charts/PDF/U2500.pdf and
https://www.unicode.org/charts/PDF/U2580.pdf. Wikipedia's summary is at
https://en.wikipedia.org/wiki/Box-drawing_characters. Where this repo and the
standard disagree, the standard wins.

## License

MIT. See [LICENSE](LICENSE). The character tables restate the Unicode Standard,
which is itself under the Unicode license.
