![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.31` · mod `0.3.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Selection | Previous: [Rating](../05-forms/12_rating_en.md) | Next: [MultiSelect&lt;T&gt;](02_multiselect_en.md) | [Русский](01_select_ru.md)

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
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `Items` | `List<SelectItem<T>>` | empty | Available options. |
| `Value` | `Func<T>` | `null` | Source of the current value. |
| `OnChange` | `Action<T>` | `null` | Selection callback. |
| `Placeholder` | `string` | `"-"` | Text shown when nothing is selected. |
| `Disabled` | `bool` | `false` | Disables the control. |
| `MaxListHeight` | `float` | `220` | Maximum panel height. |
| `ItemHeight` | `float` | `26` | Option row height. |
| `FieldHeight` | `float` | `28` | Trigger field height. |
| `Searchable` | `bool` | `false` | Shows a search field using LIKE `%..%` matching. |
| `Placement` | `OverlayPlacement` | `Bottom` | Which side the panel opens on: `Bottom`, `Top`, `Right`, `Left`, `Auto` (downwards, or upwards when there is not enough room). |
| `Align` | `OverlayAlign` | `Start` | Alignment along the other axis: `Start`, `Center`, `End`, `Auto`. `End` pins the panel's right edge to the field's right edge, so it runs to the left. |
| `PanelWidth` | `PanelWidthMode` | `Field` | Panel width: `Field` - the same as the field, `Content` - exactly as wide as the longest item, narrower than the field included. |
| `MaxPanelWidth` | `float` | `0` | Width cap in `Content` mode; `0` = the window's width is the only limit. |
| `CaseSensitive` | `bool` | `false` | Enables case-sensitive search. |
| `SearchPlaceholder` | `string` | `"Search..."` | Search-field placeholder. |

`SelectItem<T>(T value, string text = null)` is the option helper class. It exposes `T Value`, `string Text`, and `int Icon`.

### Exact panel placement

The panel opens below the field (`OverlayPlacement.Bottom`) with a 2 px gap. It flips above when it does not fit below but does fit above, using the same algorithm as `Popover`. Height is based on the actual number of visible options after search filtering: `visible height = min(visible items * ItemHeight, MaxListHeight)`. Internal scrolling appears when that list exceeds `MaxListHeight`.


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

`SelectItem.Content` takes any composition instead of a caption: a mod's picture, a row with a tag,
a progress bar. The item is laid out as an ordinary element, so events, the cursor and nested
controls work inside it.

```csharp
var row = new FlexBox(Axis.Row) { Style = { Gap = 6f, AlignItems = AlignItems.Center } };
row.Add(new Text("Plasteel"));
row.Add(new Tag("rare", Severity.Warn));

sel.Items.Add(new SelectItem<string>("plasteel", "Plasteel") { Content = row });
```

**`Text` is still not redundant**: search in the list goes by it. Leave it out and the item will
not be found by search.

**The element belongs to its item**: one per item. The same object cannot be put into two items -
an element has a single place in the tree.

The selected item is shown by its content in the field as well, by the same object. While the list
is open it therefore appears in the tree twice: it is drawn correctly in both places, while state
(the highlight, say) is shared between them.

### What you can put there

The table lists what the showcase demonstrates and what has been verified. Beyond it the behaviour
is not guaranteed:

| Content | Behaviour |
|---|---|
| `Text`, `Image`, `Tag`, `Chip`, `ProgressBar` | drawn as is; the row height is measured from it |
| `Field` / `FlexBox` made of the above | laid out in the usual way |
| `Button`, `Checkbox`, `RadioButton`, `ToggleSwitch` | the control takes the click; the item is selected by clicking the row's background |
| Anything else: text fields, nested dropdowns, `ScrollBox`, drag-and-drop zones | not verified and not supported |


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

[Table of contents](../index_en.md) - Selection | Previous: [Rating](../05-forms/12_rating_en.md) | Next: [MultiSelect&lt;T&gt;](02_multiselect_en.md) | [Русский](01_select_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
