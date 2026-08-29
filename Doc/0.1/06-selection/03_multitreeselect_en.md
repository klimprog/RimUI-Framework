![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Selection | Previous: [MultiSelect&lt;T&gt;](02_multiselect_en.md) | Next: [SelectButton&lt;T&gt;](04_selectbutton_en.md) | [Русский](03_multitreeselect_ru.md)

---

# MultiTreeSelect

The tree-backed counterpart to `MultiSelect`. Only leaves are selectable by default. Clicking a leaf toggles it, selected leaves appear as chips, and search matches leaf names.

`RimUI.Components.MultiTreeSelect : UiElement`

## Example

```csharp
var mts = new MultiTreeSelect { Placeholder = "Leaves", Searchable = true };
mts.TreePanel.Nodes.Add(new TreeItem("Weapons")
    .Sub(new TreeItem("Ranged").Sub(new TreeItem("Rifle")).Sub(new TreeItem("Bow")))
    .Sub(new TreeItem("Melee").Sub(new TreeItem("Sword"))));
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `TreePanel` | `Tree` (readonly) | new `Tree()` | Option tree. Add nodes through `TreePanel.Nodes`. |
| `OnChange` | `Action<IList<TreeItem>>` | `null` | Receives the full selected-node collection. |
| `Placeholder` | `string` | `"-"` | Text shown when nothing is selected. |
| `LeavesOnly` | `bool` | `true` | Allows selection of leaves only; branches only expand or collapse. |
| `Disabled` | `bool` | `false` | Disables the control. |
| `MaxSelected` | `int` | `0` (unlimited) | Maximum selected leaves. |
| `FieldHeight` | `float` | `28` | Trigger field height. |
| `PanelHeight` | `float` | `240` | Tree panel height. |
| `Searchable` | `bool` | from `TreePanel` | Proxies `TreePanel.Searchable`. |
| `CaseSensitive` | `bool` | from `TreePanel` | Proxies `TreePanel.CaseSensitive`. |

The panel opens below and flips above like `Select`. Search reuses `Tree.CollectFiltered` and matches leaves only; see `Tree` for exact behavior.

## Styles

Uses `select/panel`, `select/field*`, and `chip/*` together with the `tree/*` slots.

## Methods

Only the parameterless `MultiTreeSelect()` constructor is public.

| Method | Returns | Description |
|---|---|---|
| `GetSelected()` | `IList<TreeItem>` | Selected nodes in selection order; empty before the first render. There is no `SetSelected`. Leaf and chip clicks mutate the list. |
| `SetStyle(string path, string value)` | - | Applies a theme-style path to the element itself. Unknown paths and invalid values are ignored. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnChange` | `Action<IList<TreeItem>>` | full selected-node collection | A tree leaf is clicked. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The pointer enters or leaves the element bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered. Keep the handler lightweight and avoid allocations, searches, or I/O. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | The mouse wheel is used over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | A leaf is added or removed after `OnChange`. Index is always `-1`; value is the complete `IList<TreeItem>`. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `select/*`, `tree/*`, and `chip/*` slots. |
| Sprite frame | Yes | Field frame and panel. |
| Animation | Yes | Shared `Style.Animation` system. |
| Disabled | Yes | Prevents the panel from opening. |
| Hover fade | No (trigger field) | The field frame switches immediately; tree rows fade smoothly. |


---

[Table of contents](../index_en.md) - Selection | Previous: [MultiSelect&lt;T&gt;](02_multiselect_en.md) | Next: [SelectButton&lt;T&gt;](04_selectbutton_en.md) | [Русский](03_multitreeselect_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
