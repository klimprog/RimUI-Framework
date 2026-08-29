![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Grid | Previous: [Getting started](../00-start/01_start_en.md) | Next: [GridCell](02_gridcell_en.md) | [Русский](01_grid_ru.md)

---

# Grid

A 12-column grid. It supports any number of rows; each column is 1/12 of the
available width. `Style.Gap` controls spacing between cells and `Style.Padding` controls the outer
inset. Cells flow from left to right and top to bottom while avoiding areas occupied by row spans.

`RimUI.Layout.Grid : UiElement`

## Example

```csharp
var grid = new Grid();
grid.Cell(6, new Text("Left half"));     // colSpan=6 of 12 -> 50% width
grid.Cell(6, new Text("Right half"));
grid.Cell(4, 2, someTallContent);        // colSpan=4, rowSpan=2
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Columns` (const) | `int` | `12` | Fixed number of grid columns. |

Apart from inherited `Style`, Grid has no configurable fields of its own. See Style: `Style.Gap`
sets the gap between cells and `Style.Padding` sets the outer inset.

## Exact auto-placement algorithm

Placement is **next-fit**, not backtracking bin packing. The row/column cursor only moves forward
and never returns to an earlier gap.

- If a cell's `ColSpan` does not fit in the remaining columns, the entire cell moves to the next
  row. The cursor resets to column 0; Grid does not search earlier positions for a fit.
- If the width fits but some target columns are occupied, usually by a `RowSpan` cell hanging down
  from a previous row, the cursor advances one column and retries. This lets a cell move around a
  row-span region within the same row.
- **This can leave holes.** If an 8-column cell is followed by a 6-column cell, the second cell
  cannot fit in the 4 columns left over and moves to a new row. Those 4 columns stay empty because
  the cursor has already passed them. Plan adjacent `ColSpan` values so they divide 12 cleanly if
  you want to avoid invisible gaps.
- For a cell with `RowSpan > 1`, if the covered row heights plus gaps are too short for the desired
  content height, the entire shortfall is added to the last covered row. It is not distributed
  proportionally across all covered rows.

## Styles

Uses regular `UiElement.Style`: background, border, radius, and spacing work like they do on any
container. Grid has no dedicated theme slot.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Cell(GridCell cell)` | `Grid` | Add an existing cell and return this Grid for chaining. |
| `Cell(int colSpan, UiElement content)` | `Grid` | Add content in a cell spanning 1..12 columns. |
| `Cell(int colSpan, int rowSpan, UiElement content)` | `Grid` | Same, with a row span. |
| `int CellCount` (property) | `int` | Number of cells in the grid. |
| `GridCell CellAt(int i)` | `GridCell` | Get a cell by insertion index. |
| `RemoveCellAt(int i)` | `void` | Remove a cell by insertion index. |
| `ClearCells()` | `void` | Remove all cells. |

Dynamic cell access (`CellAt`/`RemoveCellAt`/`ClearCells`) is useful for visual UI editors such as
the builder included in the mod settings.

## Events

Grid has no component-specific events or callbacks.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style (`Style`) | Yes | Regular container style: background, border, padding, and gap. |
| Sprite frame | Yes | Through `Style.Sprite`, like any other element. |
| Animation (`Style.Animation`) | Yes | Uses the shared `UiElement.EmitStyled` path. |
| Sprite tiling | Yes | When `Style.Sprite.Repeat` is enabled. |
| Disabled | No | A grid has no disabled state. |
| Hover fade | No | Grid itself does not react to hover. |
| Drag and drop | Indirectly | Implemented by its `GridCell` children; see GridCell. |


---

[Table of contents](../index_en.md) - Grid | Previous: [Getting started](../00-start/01_start_en.md) | Next: [GridCell](02_gridcell_en.md) | [Русский](01_grid_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
