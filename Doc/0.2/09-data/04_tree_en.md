![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.9` · mod `0.2.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Data | Previous: [PickList&lt;T&gt;](03_picklist_en.md) | Next: [DataTable](05_datatable_en.md) | [Русский](04_tree_ru.md)

---

# Tree

A tree with expandable branches, optional multi-selection, leaf search, and tri-state checkboxes that cascade through descendants.

`RimUI.Components.Tree : UiElement`

## Example

```csharp
var tr = new Tree { MultiSelect = true, Searchable = true, Style = { Height = 200f } };
tr.Nodes.Add(new TreeItem("Base")
    .Sub(new TreeItem("Storage")
        .Sub(new TreeItem("Food") { Icon = Icons.Dot })
        .Sub(new TreeItem("Medicine"))));
tr.OnSelect = n => { /* selected node */ };

var trc = new Tree { Checkable = true };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Nodes` | `List<TreeItem>` (readonly) | empty | Root nodes. |
| `Checkable` | `bool` | `false` | Enables unchecked, partial, and checked states. |
| `MultiSelect` | `bool` | `false` | Enables row multi-selection. |
| `RowHeight` | `float` | `24` | Row height. |
| `Indent` | `float` | `18` | Indent per nesting level. |
| `Disabled` | `bool` | `false` | Disables interaction. |
| `Searchable` | `bool` | `false` | Searches leaves and shows matching leaves with ancestor context. |
| `CaseSensitive` | `bool` | `false` | Enables case-sensitive search. |
| `SearchPlaceholder` | `string` | `"Search..."` | Search-field placeholder. |

`TreeItem(string text, object value = null)` exposes `Key`, `Text`, `Icon = -1`, `Value`, `Children`, and `HasChildren`. Fluent `Sub(TreeItem child)` adds a child.

### Exact search behavior

Only leaves (`!n.HasChildren`) are compared with the filter. A branch remains only when its subtree contains a matching leaf. While search is active, `Expanded` is not changed; visible context branches are merely drawn open. Clearing search restores the exact prior expansion state. Matching is a case-insensitive substring by default, equivalent to LIKE `%filter%`.

## Styles

Slots: `tree/frame`, `tree/row`, `tree/row_hover`, `tree/row_selected`, `tree/scrollbar`, `tree/chevron`, `tree/check`, `tree/check_on`, and `tree/check_mark`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `FocusSearch()` | `void` | Focuses search on the next frame. |
| `St(UiState s)` | `TreeState` | Returns internal `Expanded`, `Checked`, `SelectedKeys`, `Scroll`, and `Filter` state. |
| `GetSelected()` | `TreeItem` | Last selected node, or last toggled node in multi-select. Returns `null` before selection. There is no `SetSelected` because mutation requires a stable node key. |
| `SetStyle(string path, string value)` | `void` | Applies the path to the root style. Unknown paths and values are ignored. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnSelect` | `Action<TreeItem>` | node | A row is clicked. |
| `OnSelectKeyed` | `Action<TreeItem, string>` | node and stable key | A node is selected. |
| `OnToggle` | `Action<TreeItem, bool>` | node and expanded state | A branch opens or closes. |
| `OnCheck` | `Action<TreeItem, bool>` | node and checked value | A checkbox is clicked; the value cascades through its subtree. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | After `OnSelect` and `OnSelectKeyed`. Index is always `-1`; value is the selected `TreeItem`. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `tree/*` slots. |
| Sprite frame | Yes | `tree/frame`. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled | Yes | Blocks row and checkbox clicks. |
| Hover fade | Yes | Stable node-key IDs survive sorting and scrolling. |


---

[Table of contents](../index_en.md) - Data | Previous: [PickList&lt;T&gt;](03_picklist_en.md) | Next: [DataTable](05_datatable_en.md) | [Русский](04_tree_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
