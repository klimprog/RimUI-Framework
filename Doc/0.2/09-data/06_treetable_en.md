![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Data | Previous: [DataTable](05_datatable_en.md) | Next: [ScrollBox](07_scrollbox_en.md) | [Русский](06_treetable_ru.md)

---

# TreeTable

A table whose first column is an expandable tree and whose remaining columns contain regular cells. Sorting reorders siblings only, preserving hierarchy; filters match leaves only.

`RimUI.Components.TreeTable : UiElement`

## Example

```csharp
TreeTableItem Node(string text, int qty, int w, int price) =>
    new TreeTableItem(text, Cell(qty.ToString()), Cell(w.ToString()), Cell(price.ToString())) { Value = qty };

var tt = new TreeTable(
    new TableColumn("Category", 0f, 2f) { SortNode = n => n.Text, FilterNode = n => n.Text },
    new TableColumn("Qty", 80f) { SortNode = n => (int)n.Value },
    new TableColumn("Weight", 80f),
    new TableColumn("Price", 80f))
{ GridLines = true, Style = { Height = 250f } };

tt.Nodes.Add(Node("Resources", 245, 310, 1200)
    .Sub(Node("Steel", 120, 180, 720))
    .Sub(Node("Wood", 95, 110, 380)));
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `Columns` | `List<TableColumn>` (readonly) | constructor input | Columns; the first is always the tree column. |
| `Nodes` | `List<TreeTableItem>` (readonly) | empty | Root nodes. |
| `Zebra` | `bool` | `true` | Alternates row backgrounds. |
| `GridLines` | `bool` | `false` | Draws grid lines. |
| `RowHeight` | `float` | `26` | Row height. |
| `HeaderHeight` | `float` | `28` | Header height. |
| `Indent` | `float` | `16` | Indent per nesting level. |
| `CaseSensitive` | `bool` | `false` | Case-sensitive filters. |

Uses `TableColumn` with `SortNode` and `FilterNode` instead of `SortBy` and `FilterBy`. `TreeTableItem` exposes `Key`, `Text`, `Icon = -1`, `Cells`, `Children`, and `Value`; fluent `Sub` adds children.

Selection is always single. Unlike `DataTable`, there is no horizontal scrolling.

### Exact sibling sorting

The active column and direction are global, but sorting runs independently and recursively at every tree level. Children may reorder only among siblings and never move to another parent. Unlike `DataTable`, sorted order is recomputed during every `Measure` frame rather than cached.

## Styles

Uses `table/*` plus `table/chevron` for tree nodes.

## Methods

| Method | Returns | Description |
|---|---|---|
| `TreeTable(params TableColumn[] columns)` (constructor) | - | Creates a tree table. |
| `St(UiState s)` | `TreeTableState` | Returns `Expanded`, `SelectedKey`, `Scroll`, `SortCol`, and `Filters`. |
| `GetSelected()` | `TreeTableItem` | Last selected node, or `null`. There is no `SetSelected`. |
| `SetStyle(string path, string value)` | `void` | Applies the path to the root style. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnSelect` | `Action<TreeTableItem>` | selected node | A row is clicked. Clicking a branch selects and expands it. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | After selection and `OnSelect`. Index is always `-1`; value is the selected `TreeTableItem`. |

`TreeTable` has no own `Disabled` field.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `table/*` plus `tree/row` for the first column. |
| Sprite frame | Yes | `table/frame`. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled | No | No disabled state. |
| Hover fade | Yes | IDs use stable node keys. |


---

[Table of contents](../index_en.md) - Data | Previous: [DataTable](05_datatable_en.md) | Next: [ScrollBox](07_scrollbox_en.md) | [Русский](06_treetable_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
