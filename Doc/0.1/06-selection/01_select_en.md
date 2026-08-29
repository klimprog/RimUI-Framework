![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Selection | Previous: [InputBase](../05-forms/11_inputbase_en.md) | Next: [MultiSelect&lt;T&gt;](02_multiselect_en.md) | [Русский](01_select_ru.md)

---

# Select&lt;T&gt;

A dropdown selector. Clicking the field opens a list; clicking an item selects it and closes the panel. Supports placeholders, scrolling and search for long lists, custom frame sprites, and a disabled state.

`RimUI.Components.Select<T> : UiElement` (generic)

## Example

```csharp
new Select<string>(new[] { "Colonist", "Alien", "Mechanoid" }, null) { Placeholder = "Choose a type" };
new Select<int>(many, null) { Placeholder = "40 items", Searchable = true };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Items` | `List<SelectItem<T>>` | empty | Available options. |
| `Value` | `Func<T>` | `null` | Source of the current value. |
| `OnChange` | `Action<T>` | `null` | Selection callback. |
| `Placeholder` | `string` | `"-"` | Text shown when nothing is selected. |
| `Disabled` | `bool` | `false` | Disables the control. |
| `MaxListHeight` | `float` | `220` | Maximum panel height. |
| `ItemHeight` | `float` | `26` | Option row height. |
| `FieldHeight` | `float` | `28` | Trigger field height. |
| `Searchable` | `bool` | `false` | Shows a search field using LIKE `%..%` matching. |
| `CaseSensitive` | `bool` | `false` | Enables case-sensitive search. |
| `SearchPlaceholder` | `string` | `"Search..."` | Search-field placeholder. |

`SelectItem<T>(T value, string text = null)` is the option helper class. It exposes `T Value`, `string Text`, and `int Icon`.

### Exact panel placement

The panel opens below the field (`OverlayPlacement.Bottom`) with a 2 px gap. It flips above when it does not fit below but does fit above, using the same algorithm as `Popover`. Height is based on the actual number of visible options after search filtering: `visible height = min(visible items * ItemHeight, MaxListHeight)`. Internal scrolling appears when that list exceeds `MaxListHeight`.

## Styles

Theme slots: `select/field`, `select/field_focus`, `select/field_disabled`, `select/panel`, `select/item`, `select/item_selected`, `select/item_hover`, `select/chevron`, and `select/scrollbar`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Select()` (constructor) | - | Creates an empty list; populate `Items` manually. |
| `Select(IEnumerable<T> values, Action<T> onChange = null)` (constructor) | - | Creates options from values. |
| `Select(IEnumerable<SelectItem<T>> items, Action<T> onChange = null)` (constructor) | - | Uses prebuilt options. |
| `GetSelected()` | `T` | Returns `Value` when supplied, otherwise the value cached during the latest frame. Before the first render this is `default(T)`, unless set through `SetSelected`. |
| `SetSelected(T value)` | - | Selects a value programmatically. Without a `Value` source, updates internal state on the next frame; always calls `OnChange`. |
| `SetStyle(string path, string value)` | - | Applies a theme-style path such as `"text.color"` or `"background"` to the element itself. Unknown paths and invalid values are ignored. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnChange` | `Action<T>` | selected value | An option is clicked. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The pointer enters or leaves the element bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered. Keep the handler lightweight and avoid allocations, searches, or I/O. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | The mouse wheel is used over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | Selection changes after the item click and `OnChange`. Index is the option's position in `Items`; value is its `Value`. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `select/*` slots. |
| Sprite frame | Yes | The field frame and panel may use slot sprites. |
| Animation | Yes | Shared `Style.Animation` system. |
| Disabled | Yes | Prevents the panel from opening. |
| Hover fade | Yes | Used by the field frame and option rows. |


---

[Table of contents](../index_en.md) - Selection | Previous: [InputBase](../05-forms/11_inputbase_en.md) | Next: [MultiSelect&lt;T&gt;](02_multiselect_en.md) | [Русский](01_select_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
