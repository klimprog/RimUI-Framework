![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.31` · mod `0.3.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Selection | Previous: [Select&lt;T&gt;](01_select_en.md) | Next: [MultiTreeSelect](03_multitreeselect_en.md) | [Русский](02_multiselect_ru.md)

---

# MultiSelect&lt;T&gt;

Selects multiple values from a list. Clicking an option adds or removes it. Selected values appear as removable chips in the field; overflow collapses into `+N`. The panel stays open after each choice and supports search and a selection limit.

`RimUI.Components.MultiSelect<T> : UiElement` (generic)

## Example

```csharp
new MultiSelect<string>(skills) { Placeholder = "Skills", Searchable = true };
new MultiSelect<string>(skills) { Placeholder = "Up to three", MaxSelected = 3 };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `Items` | `List<SelectItem<T>>` | empty | Available options. |
| `Values` | `Func<ICollection<T>>` | `null` | Source collection. When supplied, the control mutates it directly. |
| `OnChange` | `Action<ICollection<T>>` | `null` | Receives the full selected collection. |
| `Placeholder` | `string` | `"-"` | Text shown when nothing is selected. |
| `Placement` | `OverlayPlacement` | `Bottom` | Which side the panel opens on: `Bottom`, `Top`, `Right`, `Left`, `Auto` (downwards, or upwards when there is not enough room). |
| `Align` | `OverlayAlign` | `Start` | Alignment along the other axis: `Start`, `Center`, `End`, `Auto`. `End` pins the panel's right edge to the field's right edge, so it runs to the left. |
| `Disabled` | `bool` | `false` | Disables the control. |
| `MaxSelected` | `int` | `0` (unlimited) | Maximum number of selected items. |
| `MaxListHeight` | `float` | `220` | Maximum panel height. |
| `ItemHeight` | `float` | `26` | Option row height. |
| `FieldHeight` | `float` | `28` | Trigger field height. |
| `Searchable` | `bool` | `false` | Shows a search field. |
| `CaseSensitive` | `bool` | `false` | Enables case-sensitive search. |
| `SearchPlaceholder` | `string` | `"Search..."` | Search-field placeholder. |

### Exact `MaxSelected` behavior

Once the limit is reached, clicking an unselected item does nothing. The panel stays open and no older selection is removed automatically. Already selected items can always be deselected; the limit applies only when adding. Panel placement and flipping match `Select`.


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

## Content of your own for an item

`SelectItem.Content` works the same as in `Select` (see its page): a composition instead of a
caption, with `Text` kept for search. The item's checkbox stays in its place.


Uses the `Select` slots (`select/field*`, `select/panel`, and `select/item*`), plus `chip`, `chip/text`, and `chip/close` for selected values.

## Methods

| Method | Returns | Description |
|---|---|---|
| `MultiSelect()` (constructor) | - | Creates an empty list. |
| `MultiSelect(IEnumerable<SelectItem<T>> items, Action<ICollection<T>> onChange = null)` (constructor) | - | Uses prebuilt options. |
| `MultiSelect(IEnumerable<T> values, Action<ICollection<T>> onChange = null)` (constructor) | - | Creates options from values. |
| `GetSelected()` | `ICollection<T>` | Returns `Values` when supplied, otherwise the collection cached during the latest frame. There is no `SetSelected`; item and chip clicks mutate the modder-owned collection. |
| `SetStyle(string path, string value)` | - | Applies a theme-style path to the element itself. Unknown paths and invalid values are ignored. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnChange` | `Action<ICollection<T>>` | full selected collection | An option or a chip close button is clicked. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The pointer enters or leaves the element bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered. Keep the handler lightweight and avoid allocations, searches, or I/O. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | The mouse wheel is used over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | An option is toggled after `OnChange`. Index identifies that option in `Items`; value is the full `ICollection<T>` after the change. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `select/*` and `chip/*` slots. |
| Sprite frame | Yes | Field frame and panel. |
| Animation | Yes | Shared `Style.Animation` system. |
| Disabled | Yes | Prevents the panel from opening. |
| Hover fade | Yes | Used by the field frame and option rows. |


---

[Table of contents](../index_en.md) - Selection | Previous: [Select&lt;T&gt;](01_select_en.md) | Next: [MultiTreeSelect](03_multitreeselect_en.md) | [Русский](02_multiselect_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
