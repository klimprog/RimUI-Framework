![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.3.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Selection | Previous: [SelectButton&lt;T&gt;](04_selectbutton_en.md) | Next: [CascadeSelect](06_cascadeselect_en.md) | [Русский](05_treeselect_ru.md)

---

# TreeSelect

A select field backed by a tree instead of a flat list. Expand branches and click a node to select it.

`RimUI.Components.TreeSelect : UiElement`

## Example

```csharp
var tsel = new TreeSelect { Placeholder = "Node", Searchable = true };
tsel.TreePanel.Nodes.Add(new TreeItem("Weapons").Sub(new TreeItem("Ranged").Sub(new TreeItem("Rifle"))));
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `TreePanel` | `Tree` (readonly) | new `Tree()` | Option tree; see `Tree`. |
| `OnChange` | `Action<TreeItem>` | `null` | Receives the entire selected node, not just its value. |
| `Placeholder` | `string` | `"-"` | Text shown when nothing is selected. |
| `LeavesOnly` | `bool` | `true` | Allows selection of leaves only. |
| `Disabled` | `bool` | `false` | Disables the control. |
| `FieldHeight` | `float` | `28` | Trigger field height. |
| `PanelHeight` | `float` | `240` | Tree panel height. |
| `Searchable` | `bool` | from `TreePanel` | Proxies `TreePanel.Searchable`. |
| `CaseSensitive` | `bool` | from `TreePanel` | Proxies `TreePanel.CaseSensitive`. |

The panel opens below and flips above like `Select`. Search fully reuses `Tree.CollectFiltered`: only leaves match, and context branches are forced open without changing their stored `Expanded` state. See `Tree` for details.

## Styles

Uses the `Select` family slots (`select/panel` and `select/field*`) together with `tree/*`.

## Methods

Only the parameterless `TreeSelect()` constructor is public.

| Method | Returns | Description |
|---|---|---|
| `GetSelected()` | `TreeItem` | Returns the selected node, or `null` before a selection unless set through `SetSelected`. |
| `SetSelected(TreeItem item)` | - | Selects a node programmatically, updates the field text, and calls `OnChange`. |
| `SetStyle(string path, string value)` | - | Applies a theme-style path to the element itself. Unknown paths and invalid values are ignored. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnChange` | `Action<TreeItem>` | selected node | A permitted node is clicked, respecting `LeavesOnly`. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The pointer enters or leaves the element bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered. Keep the handler lightweight and avoid allocations, searches, or I/O. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | The mouse wheel is used over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | A leaf is selected after `OnChange`. Index is always `-1`; value is the selected `TreeItem`. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `select/*` and `tree/*` slots. |
| Sprite frame | Yes | Field frame and panel. |
| Animation | Yes | Shared `Style.Animation` system. |
| Disabled | Yes | Prevents the panel from opening. |
| Hover fade | No (trigger field) | The field switches immediately; tree rows fade smoothly. |


---

[Table of contents](../index_en.md) - Selection | Previous: [SelectButton&lt;T&gt;](04_selectbutton_en.md) | Next: [CascadeSelect](06_cascadeselect_en.md) | [Русский](05_treeselect_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
