# Junction lookup

Given the arms that meet at a cell and the weight of each, find the character.
This is the table to consult while drawing. For the full inventory see
[charset.md](charset.md).

Arms are named `U`, `R`, `D`, `L` (up, right, down, left). A junction is
determined entirely by its four arms: each is absent, light, heavy, or double.

## Method

1. List the arms that actually leave the cell. Arms are what touches the cell,
   not what the shape looks like.
2. Note each arm's weight.
3. If all arms share a weight, use the uniform table below.
4. If weights are mixed, use the light/heavy table for the matching family, or
   the single/double table.
5. If the mix is heavy with double, no character exists. Change one line.

## Uniform junctions

| Arms    | Light | Heavy | Double | Rounded |
|---------|-------|-------|--------|---------|
| D R     | `┌`   | `┏`   | `╔`    | `╭`     |
| D L     | `┐`   | `┓`   | `╗`    | `╮`     |
| U R     | `└`   | `┗`   | `╚`    | `╰`     |
| U L     | `┘`   | `┛`   | `╝`    | `╯`     |
| U R D   | `├`   | `┣`   | `╠`    | none    |
| U L D   | `┤`   | `┫`   | `╣`    | none    |
| L R D   | `┬`   | `┳`   | `╦`    | none    |
| L R U   | `┴`   | `┻`   | `╩`    | none    |
| U R D L | `┼`   | `╋`   | `╬`    | none    |
| L R     | `─`   | `━`   | `═`    | same    |
| U D     | `│`   | `┃`   | `║`    | same    |

## Light and heavy mixed

Every combination exists. The tables list which arms are heavy; all other
present arms are light.

### Corners

| Heavy arms | D R | D L | U R | U L |
|------------|-----|-----|-----|-----|
| none       | `┌` | `┐` | `└` | `┘` |
| horizontal | `┍` | `┑` | `┕` | `┙` |
| vertical   | `┎` | `┒` | `┖` | `┚` |
| both       | `┏` | `┓` | `┗` | `┛` |

### T-junctions

Each column is one family. The row is the set of heavy arms within that family.

| Heavy arms | `├` family (U R D) | `┤` family (U L D) |
|------------|--------------------|--------------------|
| none       | `├`                | `┤`                |
| stem only  | `┝` (R)            | `┥` (L)            |
| U          | `┞`                | `┦`                |
| D          | `┟`                | `┧`                |
| U D        | `┠`                | `┨`                |
| U + stem   | `┡`                | `┩`                |
| D + stem   | `┢`                | `┪`                |
| all        | `┣`                | `┫`                |

| Heavy arms | `┬` family (L R D) | `┴` family (L R U) |
|------------|--------------------|--------------------|
| none       | `┬`                | `┴`                |
| L          | `┭`                | `┵`                |
| R          | `┮`                | `┶`                |
| L R        | `┯`                | `┷`                |
| stem only  | `┰` (D)            | `┸` (U)            |
| L + stem   | `┱`                | `┹`                |
| R + stem   | `┲`                | `┺`                |
| all        | `┳`                | `┻`                |

### Cross

| Heavy arms | Glyph | Heavy arms | Glyph |
|------------|-------|------------|-------|
| none       | `┼`   | R U        | `╄`   |
| L          | `┽`   | L D        | `╅`   |
| R          | `┾`   | R D        | `╆`   |
| L R        | `┿`   | L R U      | `╇`   |
| U          | `╀`   | L R D      | `╈`   |
| D          | `╁`   | U D L      | `╉`   |
| U D        | `╂`   | U D R      | `╊`   |
| L U        | `╃`   | all        | `╋`   |

## Single and double mixed

Double style is chosen per axis, never per arm. Both arms of the horizontal share
a style, and both arms of the vertical share a style.

| Arms    | H double, V single | H single, V double | Both double |
|---------|--------------------|--------------------|-------------|
| D R     | `╒`                | `╓`                | `╔`         |
| D L     | `╕`                | `╖`                | `╗`         |
| U R     | `╘`                | `╙`                | `╚`         |
| U L     | `╛`                | `╜`                | `╝`         |
| U R D   | `╞`                | `╟`                | `╠`         |
| U L D   | `╡`                | `╢`                | `╣`         |
| L R D   | `╤`                | `╥`                | `╦`         |
| L R U   | `╧`                | `╨`                | `╩`         |
| U R D L | `╪`                | `╫`                | `╬`         |

At a corner or T, "H double, V single" means the horizontal arms present are
double. `╒` has a double right arm and a single down arm.

## What does not exist

- Heavy meeting double at any junction. Not in Unicode, in any combination.
- Double meeting double at different weights. Double has one weight.
- Rounded T-junctions and rounded crosses. Arcs are corners only.
- Rounded or double weight-transition characters. Only `╼ ╽ ╾ ╿` exist, light to
  heavy.
- Corners or junctions in any dashed style.
- Per-arm double asymmetry, for example a double left arm with a single right arm
  on one horizontal line.

When a drawing calls for one of these, the drawing is wrong, not Unicode. Move
the weight change one cell away from the junction, or unify the style across the
frame.

## Transitions along a line

A weight change between two junctions uses the transition glyphs. The change
happens inside a single cell.

```
├─────╼━━━━━┫
```

Left of the transition the line is light, right of it heavy, and the endpoints
match: `├` has a light right arm, `┫` has a heavy left arm. Getting the endpoints
wrong is the usual defect, and it shows as a visible notch.

For a weight change at a junction rather than between junctions, use the mixed
junction directly. A light table body under a heavy header row joins at `┡ ╇ ┩`:

```
┏━━━━━━━┳━━━━━━━┓
┃ Head  ┃ Head  ┃
┡━━━━━━━╇━━━━━━━┩
│ cell  │ cell  │
└───────┴───────┘
```

`┡` is up heavy, right heavy, down light. `╇` is up, left, and right heavy with
down light. `┩` is up heavy, left heavy, down light. Read the names and the row
verifies itself.

## Single-cell walls

A cell whose neighbors are all empty still needs the right character. Use the
stubs from [charset.md](charset.md): `╴ ╵ ╶ ╷` light and
`╸ ╹ ╺ ╻` heavy. A line that stops without a stub leaves a half-cell of
whitespace where the reader expects an end cap.
