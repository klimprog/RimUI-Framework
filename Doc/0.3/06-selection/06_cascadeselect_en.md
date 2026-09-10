![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.31` · mod `0.3.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Selection | Previous: [TreeSelect](05_treeselect_en.md) | Next: [ColorPicker](07_colorpicker_en.md) | [Русский](06_cascadeselect_ru.md)

---

# CascadeSelect

A cascading selector built on `DropdownMenu`. Hovering an item with children opens the next submenu to the side; clicking a leaf completes the selection.

`RimUI.Components.CascadeSelect : UiElement`

## Example

```csharp
var cas = new CascadeSelect(null) { Placeholder = "Region" };
cas.Items.Add(new CascadeItem("Temperate").Sub(new CascadeItem("Forest")).Sub(new CascadeItem("Hills")));
cas.Items.Add(new CascadeItem("Harsh").Sub(new CascadeItem("Tundra")));
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `Items` | `List<CascadeItem>` | empty | Root cascade items. |
| `OnChange` | `Action<object>` | `null` | Receives the selected leaf's `CascadeItem.Value`. |
| `Placeholder` | `string` | `"-"` | Text shown when nothing is selected. |
| `Disabled` | `bool` | `false` | Disables the control. |
| `FieldHeight` | `float` | `28` | Trigger field height. |
| `Clearable` | `bool` | `false` | Adds a "nothing selected" first item that clears the selection. |
| `ClearText` | `string` | `"- nothing selected -"` | Text of the clearing item. |
| `SelectBranches` | `bool` | `false` | Allows selecting a **branch**, not only a leaf. The branch stays expandable: the submenu opens on hover, while a click on the branch itself selects it. |
| `MultiSelect` | `bool` | `false` | Multi-select: a click adds an item to the set or removes it, the menu stays open, and selected items are marked with a check. |
| `MultiSummary` | `Func<int,string>` | `null` | The field's caption in multi-select mode; "selected: N" by default. |
| `CascadeItem.Content` | `UiElement` | `null` | Content of your own for an item - passed to the menu item as is. |

`CascadeItem(string text, object value = null)` defaults `value` to `text`. The fluent `Sub(CascadeItem child)` method adds a child.

Each submenu opens flush to the right of its parent item. It flips left when there is not enough room on the right but enough on the left. This is inherited from `DropdownMenu` and cannot be disabled separately.

## Styles

The trigger field uses `select/field*`. The panel uses `DropdownMenu` menu slots.

## Methods

| Method | Returns | Description |
|---|---|---|
| `CascadeSelect(Action<object> onChange = null)` (constructor) | - | Creates a cascading selector with a callback. |
| `GetSelected()` | `object` | The last selected value (`null` before the first selection). In multi-select mode, the one toggled last. |
| `GetSelectedValues()` | `List<object>` | The whole set of selected values under `MultiSelect` (an empty list when nothing is selected). |
| `SetSelected(object value)` | - | Sets the selection from code: the field's caption is taken from the item with that value. A value that is not in the list simply clears the selection. |
| `Clear()` | - | Clears the selection: the field goes back to `Placeholder`. |
| `Rebuild()` | - | Rebuilds the menu from the current list. Usually unnecessary — the contents are tracked automatically; call it when only the **texts** changed while the number of items stayed the same. |
| `SetStyle(string path, string value)` | - | Applies a theme-style path to the element itself. Unknown paths and invalid values are ignored. |

## A list that changes

The menu is rebuilt whenever the contents of `Items` change, so the list can be filled or rebuilt
(by a filter, say) after the first frame — new items become available straight away. It is the
**contents** that are tracked; if only the texts changed while the number of items stayed the
same, say so explicitly with `Rebuild()`.

```csharp
var cs = new CascadeSelect { Key = "region", Clearable = true };
cs.Items.Add(new CascadeItem("Materials", "materials")
    .Sub(new CascadeItem("Steel", "steel"))
    .Sub(new CascadeItem("Wood", "wood")));

cs.SetSelected("steel");              // the caption is taken from the item itself
```

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnChange` | `Action<object>` | leaf `CascadeItem.Value` | The final item in a cascade is clicked. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The pointer enters or leaves the element bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered. Keep the handler lightweight and avoid allocations, searches, or I/O. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | The mouse wheel is used over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | A leaf is selected after `OnChange`. Index is always `-1`; value is the selected leaf's `CascadeItem.Value`. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Field frame and panel menu slots. |
| Sprite frame | Yes | Trigger field frame. |
| Animation | Yes | Shared `Style.Animation` system. |
| Disabled | Yes | Only the field is drawn; the menu does not open. |
| Hover fade | No (trigger field) | The field frame switches immediately. |


---

[Table of contents](../index_en.md) - Selection | Previous: [TreeSelect](05_treeselect_en.md) | Next: [ColorPicker](07_colorpicker_en.md) | [Русский](06_cascadeselect_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
