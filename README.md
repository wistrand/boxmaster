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
├── SKILL.md              rules, method, quick reference
├── CLAUDE.md             how to work on the skill
└── references
    ├── junctions.md      arms in, character out
    ├── layout.md         construction recipes and sizing math
    ├── charset.md        the complete inventory
    └── gotchas.md        width, font, and terminal traps
```

## Source of truth

The Unicode Standard, code charts for Box Drawing (U+2500–U+257F) and Block
Elements (U+2580–U+259F): https://www.unicode.org/charts/PDF/U2500.pdf and
https://www.unicode.org/charts/PDF/U2580.pdf. Wikipedia's summary is at
https://en.wikipedia.org/wiki/Box-drawing_characters. Where this repo and the
standard disagree, the standard wins.

## License

No license chosen yet. The character tables restate the Unicode Standard, which
is itself under the Unicode license.
