![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Forms | Previous: [Label](01_label_en.md) | Next: [RadioButton](03_radiobutton_en.md) | [Русский](02_checkbox_ru.md)

---

# Checkbox

A checkbox toggles a boolean value. State can live inside the element or come from your code;
disabled state and theme sprites are supported.

`RimUI.Components.Checkbox : UiElement`

## Example

```csharp
new Checkbox("Enabled", v => myFlag = v) { Key = "chk1", Checked = () => myFlag };
new Checkbox("Disabled") { Disabled = true };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `LabelText` | `string` | `null` | Label text. |
| `LabelElement` | `Text` | `null` | Prepared label element. |
| `Checked` | `Func<bool>` | `null` | Value source; without one, state is stored internally by element ID. |
| `OnChange` | `Action<bool>` | `null` | Change callback. |
| `Disabled` | `bool` | `false` | Disabled state. |
| `BoxSize` | `float` | `0` (theme `checkbox/box`, default 18) | Box size. |
| `Gap` | `float` | `0` (theme, default 8) | Gap between box and label. |
| `BoxStyle`, `BoxHoverStyle`, `BoxOnStyle`, `BoxDisabledStyle`, `CheckStyle`, `LabelStyle` | read-only `Style` | — | Explicit part styles overriding theme slots. |

**The entire row is clickable**, including box, gap, and label, not just the square. Hit testing uses
the component's full `Bounds`; `Measure` already includes `BoxSize + Gap + LabelWidth`. Clicking
the caption is equivalent to clicking the box.

## Styles

Theme slots: `checkbox/box`, `checkbox/box_on`, `checkbox/box_hover`, `checkbox/check`, and
`checkbox/label`. The `checkbox/box_on`/`box_off` sprite is drawn as a complete image, not a
9-slice frame.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Checkbox(string label = null, Action<bool> onChange = null)` (constructor) | — | Create a labeled checkbox with an optional callback. |
| `GetValue()` | `bool` | Current value from `Checked`, or the previous frame's internal cache. Before first display, returns `false` or a value supplied through `SetValue`. |
| `SetValue(bool v)` | — | Set the value as if clicked: updates internal state on the next render frame when `Checked` is absent and invokes `OnChange`. With a `Checked` source, your code remains the source of truth and should update it from `OnChange`. |
| `SetStyle(string path, string value)` | — | Set a style part by string: `box`, `box_hover`, `box_on`, `box_disabled`, `check`, or `label`, for example `SetStyle("box.borderColor", "#FF5050")`. Without a part, applies to root Style. Typos and invalid values are silently ignored. |

## Events

| Event | Type | Parameters | Fired when |
|---|---|---|---|
| `OnChange` | `Action<bool>` | new value | A non-disabled checkbox is clicked or `SetValue` is called. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered, roughly 60 times per second. Avoid allocations, searches, and I/O here. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective `Disabled` value changed between frames. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Part styles plus theme slots. |
| Sprite frame | Yes | `checkbox/box_on`/`box_off`. |
| Animation | Yes | Shared `Style.Animation` mechanism. |
| Disabled | Yes | Blocks clicks and uses `BoxDisabledStyle`. |
| Hover fade | Yes | Smooth box-border transition using `Theme.HoverFadeDuration`. |


---

[Table of contents](../index_en.md) - Forms | Previous: [Label](01_label_en.md) | Next: [RadioButton](03_radiobutton_en.md) | [Русский](02_checkbox_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
