![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — core `0.6.7` · mod `0.1.0` · RimWorld `1.6`

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
| `Items` | `List<SelectItem<T>>` | empty | Available options. |
| `Values` | `Func<ICollection<T>>` | `null` | Source collection. When supplied, the control mutates it directly. |
| `OnChange` | `Action<ICollection<T>>` | `null` | Receives the full selected collection. |
| `Placeholder` | `string` | `"-"` | Text shown when nothing is selected. |
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

## Styles

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
