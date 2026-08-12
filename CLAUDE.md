Guidance for working on this repository. Read this before editing any file here.

## What this repo is

This directory is an Agent Skill. `SKILL.md` and `references/` are the skill;
`CLAUDE.md` and `README.md` are repo furniture and are not part of what the skill
teaches. Install it by symlinking this directory into `~/.claude/skills/boxmaster`.

| Path          | Role                                                           |
|---------------|----------------------------------------------------------------|
| `SKILL.md`    | skill entry point: frontmatter, rules, method, quick reference |
| `references/` | deep dives, read on demand by an agent using the skill         |
| `CLAUDE.md`   | this file: how to work on the skill                            |
| `README.md`   | human-facing overview and install instructions                 |

The subject is Unicode box drawing. The authoritative source is the Unicode
Standard, code charts U+2500 and U+2580, not this repo. On a conflict the
standard wins and the file gets fixed.

## Rules for editing this repo

- Never add executable code to the skill. No scripts, no runnable files, no
  snippets meant to be pasted and run. Every rule lives in readable markdown.
- Never let `SKILL.md` grow past roughly 500 lines. Detail belongs in
  `references/`, linked one level deep.
- Never state a rule in both `SKILL.md` and a reference file. Duplicated rules
  drift. Reference files carry subsystem detail; cross-cutting rules live in
  `SKILL.md` only.
- Always keep each reference file self-contained. An agent opens one of them
  without reading the other three.
- Always link reference files with markdown links, never bare backticks.
  Backticks are for codepoints, glyphs, and paths.
- Always align the source of every markdown table in this repo by the rules in
  [references/markdown-tables.md](references/markdown-tables.md). The files here
  are specimens of what the skill teaches.

## Verify every figure you add

Every example in these files is also a specimen of the rules. A misaligned
example teaches the error, so check any figure before saving it:

- Box rows equal in display width, vertical lines in the same column on every row
  they cross. Trees and connector diagrams may be ragged on the right; columns
  may never drift.
- Junctions match the arms that actually touch them, checked against
  [references/junctions.md](references/junctions.md), not by eye.
- No trailing whitespace on any line.

Three examples are ragged by construction and correct that way. Never straighten
them:

- the tree listing and the drop shadow in [references/layout.md](references/layout.md);
- the ragged-source table in
  [references/markdown-tables.md](references/markdown-tables.md), which exists to
  show what the rule prevents.

## The frontmatter

`SKILL.md` frontmatter carries the whole trigger decision, so treat it as load
bearing when editing.

- `name`: lowercase letters, numbers, and hyphens; 64 characters maximum; cannot
  contain "claude" or "anthropic".
- `description`: 1024 characters maximum, and must say both what the skill does
  and when to use it. It is the only part of the skill in context until the skill
  fires, so it has to name the surface forms a user will actually type: tables,
  trees, diagrams, framed text, terminal mockups, misaligned columns.

Never remove trigger vocabulary from the description to make it shorter.

## Writing style

- No AI-isms: no "powerful", "seamlessly", "leverage", rule-of-three cadences,
  "not just X but Y". No em dashes, no emojis.
- State each rule on its own line as a flat always or never. A rule buried
  mid-paragraph gets skipped.
- Concise; assume the reader is competent. Add only what cannot be inferred.
- Write codepoints as `U+2500`, uppercase hex, paired with the glyph on first
  mention.
- Name characters by their Unicode names when precision matters. The name encodes
  the arms, which is what makes it checkable.
- Mark inferred claims and open questions. Never present a guess as a fact.
