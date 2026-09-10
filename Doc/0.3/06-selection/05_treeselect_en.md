![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.31` · mod `0.3.1` · RimWorld `1.6`

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
| `Placement` | `OverlayPlacement` | `Bottom` | Which side the panel opens on: `Bottom`, `Top`, `Right`, `Left`, `Auto` (downwards, or upwards when there is not enough room). |
| `Align` | `OverlayAlign` | `Start` | Alignment along the other axis: `Start`, `Center`, `End`, `Auto`. `End` pins the panel's right edge to the field's right edge, so it runs to the left. |
| `LeavesOnly` | `bool` | `true` | Allows selection of leaves only. |
| `Disabled` | `bool` | `false` | Disables the control. |
| `FieldHeight` | `float` | `28` | Trigger field height. |
| `PanelHeight` | `float` | `240` | Tree panel height. |
| `Searchable` | `bool` | from `TreePanel` | Proxies `TreePanel.Searchable`. |
| `CaseSensitive` | `bool` | from `TreePanel` | Proxies `TreePanel.CaseSensitive`. |

The panel opens below and flips above like `Select`. Search fully reuses `Tree.CollectFiltered`: only leaves match, and context branches are forced open without changing their stored `Expanded` state. See `Tree` for details.


## Where the panel opens

The direction is set along two independent axes: `Placement` - which side of the field the panel
sits on, `Align` - how it lines up along the other axis. The pair covers all eight combinations:

| What you want | How to set it |
|---|---|
| downwards, left edges aligned | `Placement = Bottom` (the default) |
| upwards | `Placement = Top` |
| **up and to the left** | `Placement = Top`, `Align = End` |
| downwards, centred on the field | `Align = Center` |
| sideways (a submenu) | `Placement = Right` or `Left` |

`Auto` on either axis means "pick it yourself": the side flips to the opposite one when there is
not enough room.

**Room is measured against the visible area, not just the window.** A list inside a table or a
scroll area opens where it can be seen. Clamping the panel to the cell's bounds is out of the
question, though - it would have nowhere to open - so it extends past them freely.

**Alignment only shows when the widths differ.** If the panel is exactly as wide as the field,
`Start`, `Center` and `End` all look the same.


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
