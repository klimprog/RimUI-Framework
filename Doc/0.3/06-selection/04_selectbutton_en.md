![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.31` · mod `0.3.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Selection | Previous: [MultiTreeSelect](03_multitreeselect_en.md) | Next: [TreeSelect](05_treeselect_en.md) | [Русский](04_selectbutton_ru.md)

---

# SelectButton&lt;T&gt;

A joined row of button segments for choosing exactly one value. Clicking a segment activates it and deactivates the rest.

`RimUI.Components.SelectButton<T> : UiElement` (generic)

## Example

```csharp
new SelectButton<string>(new[] { "Day", "Week", "Month" }, null);
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `Items` | `List<SelectItem<T>>` | empty | Segments. |
| `Value` | `Func<T>` | `null` | Source of the current value. |
| `OnChange` | `Action<T>` | `null` | Selection callback. |
| `Disabled` | `bool` | `false` | Disables the control. |
| `SegmentHeight` | `float` | `28` | Segment height. |
| `SegmentPadding` | `float` | `12` | Horizontal padding inside each segment. |

## Styles

Theme slots: `selectbutton/segment`, `selectbutton/segment_active`, `selectbutton/segment_hover`, `selectbutton/segment_disabled`, `selectbutton/text`, and `selectbutton/text_disabled`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `SelectButton()` (constructor) | - | Creates an empty row; populate `Items` manually. |
| `SelectButton(IEnumerable<T> values, Action<T> onChange = null)` (constructor) | - | Creates segments from values. |
| `GetSelected()` | `T` | Returns `Value` when supplied, otherwise the latest cached value. Before the first render this is `default(T)` unless set through `SetSelected`. |
| `SetSelected(T value)` | - | Selects a segment programmatically. Without `Value`, updates internal state on the next frame; always calls `OnChange`. |
| `SetStyle(string path, string value)` | - | Applies a theme-style path to the element itself. Unknown paths and invalid values are ignored. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnChange` | `Action<T>` | selected value | A segment is clicked. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The pointer enters or leaves the element bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered. Keep the handler lightweight and avoid allocations, searches, or I/O. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | The mouse wheel is used over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | Selection changes after `OnChange`. Index identifies the segment in `Items`; value is its `Value`. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `selectbutton/*` slots. |
| Sprite frame | Yes | Through `Style.Background` or the segment slot sprite. |
| Animation | Yes | Shared `Style.Animation` system. |
| Disabled | Yes | Uses `segment_disabled`. |
| Hover fade | No | `segment_hover` switches immediately. |


---

[Table of contents](../index_en.md) - Selection | Previous: [MultiTreeSelect](03_multitreeselect_en.md) | Next: [TreeSelect](05_treeselect_en.md) | [Русский](04_selectbutton_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
