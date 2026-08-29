![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Data | Previous: [Tree](04_tree_en.md) | Next: [TreeTable](06_treetable_en.md) | [Русский](05_datatable_ru.md)

---

# DataTable

A table with zebra rows, hover highlighting, multi-selection, sortable headers, AND-combined column filters, and live UI elements inside cells.

`RimUI.Components.DataTable : UiElement`

## Example

```csharp
var lvls = new List<int>();
var table = new DataTable(
    new TableColumn("Colonist", 0f, 2f) { FilterBy = r => names[r] },
    new TableColumn("Skill", 0f, 1.4f) { FilterBy = r => skills[r % skills.Length] },
    new TableColumn("Level", 46f) { SortBy = r => lvls[r] },
    new TableColumn("Progress", 0f, 1.6f),
    new TableColumn("On", 50f))
{ MultiSelect = true, GridLines = true, Style = { Height = 240f } };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Columns` | `List<TableColumn>` (readonly) | constructor input | Columns. |
| `Rows` | `List<List<UiElement>>` (readonly) | empty | Rows of cell elements. |
| `Selectable` | `bool` | `true` | Enables row selection. |
| `MultiSelect` | `bool` | `false` | Enables multiple selected rows. |
| `Zebra` | `bool` | `true` | Alternates row backgrounds. |
| `GridLines` | `bool` | `false` | Draws lines between cells. |
| `RowHeight` | `float` | `28` | Row height. |
| `HeaderHeight` | `float` | `28` | Header height. |
| `CaseSensitive` | `bool` | `false` | Case-sensitive filters. |
| `MinWeightColumnWidth` | `float` | `90` | Aggregate minimum used for weighted columns before horizontal scrolling. |

`TableColumn(string title, float width = 0f, float weight = 1f)` uses fixed pixels when `width > 0`; otherwise it receives a share of remaining width by `weight`. `SortBy` returns an `IComparable` key by source-row index. `FilterBy` returns filter text; `null` disables that column's filter field.

## Styles

Slots: `table/frame`, `header`, `header_text`, `row_odd`, `row_hover`, `row_selected`, `gridline`, `scrollbar`, `sort_arrow`, `hscroll_track`, and `hscroll_handle`/`_active`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `DataTable(params TableColumn[] columns)` (constructor) | - | Creates a table. |
| `Bind<T>(IEnumerable<T> data, Func<T, UiElement[]> rowBuilder)` | `DataTable` | Rebuilds `Rows`. Call when data changes, not every frame. |
| `AddRow(params UiElement[] cells)` | `DataTable` | Adds a row and returns this table. |
| `St(UiState s)` | `TableState` | Returns state such as `Selected`, `Scroll`, `SortCol`, `Order`, and `Filters`. |
| `GetSelected()` | `HashSet<int>` | Selected source-row indices; empty before first render. There is no `SetSelected`. |
| `SetStyle(string path, string value)` | `void` | Applies the path to the root style. |

Sortable headers cycle ascending, descending, then unsorted. Nonempty filters are combined with AND and short-circuit on the first mismatch. Selection remains keyed by original row index after sorting.

### Exact weighted-column sizing

```text
contentW = max(availableWidth, fixedSum + weightCount * MinWeightColumnWidth)
rest     = contentW - fixedSum
colW[i]  = rest * (Weight[i] / weightSum)
```

`MinWeightColumnWidth` guarantees only the aggregate width of weighted columns, not each column. With weights 1 and 3 and `rest = 180`, widths are 45 and 135 px. Horizontal scrolling begins when `contentW > visibleWidth + 0.5`.

### Exact sorting behavior

Sorting is explicitly stable: equal keys retain source order by comparing their indices. `null` is less than any non-null key and sorts first in ascending order. Incompatible `IComparable` key types are not caught and will throw.

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnRowClick` | `Action<int>` | source-row index | A row is clicked. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | After selection and `OnRowClick`. Index is the source row; value is always `null`. |

`DataTable` has no own `Disabled` field.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `table/*` slots. |
| Sprite frame | Yes | `table/frame`. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled | No | No disabled state. |
| Hover fade | Yes | IDs use source-row indices and survive sorting. |


---

[Table of contents](../index_en.md) - Data | Previous: [Tree](04_tree_en.md) | Next: [TreeTable](06_treetable_en.md) | [Русский](05_datatable_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
