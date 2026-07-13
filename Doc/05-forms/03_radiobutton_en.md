![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — core `0.6.7` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Forms | Previous: [Checkbox](02_checkbox_en.md) | Next: [ToggleSwitch](04_toggleswitch_en.md) | [Русский](03_radiobutton_ru.md)

---

# RadioButton

Radio buttons share one group value. Clicking one selects its value and clears the others in that
group. The circle and inner dot can use theme sprites.

`RimUI.Components.RadioButton : UiElement`

## Example

```csharp
row.Add(new RadioButton("Easy", 1, v => _mode = (int)v) { Selected = () => _mode });
row.Add(new RadioButton("Normal", 2, v => _mode = (int)v) { Selected = () => _mode });
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `LabelText` | `string` | — | Label text. |
| `LabelElement` | `Text` | `null` | Prepared label element. |
| `Value` | `object` | — | Value represented by this button. |
| `Selected` | `Func<object>` | `null` | Current group value; share this source across buttons in the group. |
| `OnSelect` | `Action<object>` | `null` | Selection callback. |
| `Disabled` | `bool` | `false` | Disabled state. |
| `CircleSize` | `float` | `0` (theme `radio/circle`, default 18) | Circle size. |
| `Gap` | `float` | `0` (theme, default 8) | Gap between circle and label. |
| `CircleStyle`, `CircleHoverStyle`, `CircleOnStyle`, `CircleDisabledStyle`, `DotStyle`, `LabelStyle` | read-only `Style` | — | Explicit part styles. |

**The entire row is clickable**, including circle, gap, and label. Hit testing uses the component's
full `Bounds`, with no extra expansion or shrinkage.

## Styles

Theme slots: `radio/circle`, `radio/circle_hover`, `radio/circle_on`, `radio/dot`, and
`radio/label`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `RadioButton(string label, object value, Action<object> onSelect = null)` (constructor) | — | `label` and `value` are required. |
| `SetStyle(string path, string value)` | — | Parts: `circle`, `circle_hover`, `circle_on`, `circle_disabled`, `dot`, and `label`; for example `SetStyle("circle.borderColor", "#FF5050")`. Unknown paths and values are silently ignored. |

RadioButton has no `GetValue`/`SetValue` or `GetSelected`/`SetSelected`. Group selection is read and
written only through `Selected` and `OnSelect`. The button stores no value state of its own; it only
compares `Selected()` with its `Value`.

## Events

| Event | Type | Parameters | Fired when |
|---|---|---|---|
| `OnSelect` | `Action<object>` | this button's `Value` | A non-disabled button is clicked. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex` is always `-1`; `data.SelectedValue` is this `Value` | After `OnSelect` on a valid click. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective `Disabled` state changes. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Circle, dot, and label styles plus theme slots. |
| Sprite frame | Yes | Circle sprite from `radio/circle*`. |
| Animation | Yes | Shared `Style.Animation` mechanism. |
| Disabled | Yes | Blocks clicks and uses `CircleDisabledStyle`. |
| Hover fade | Yes | Smooth circle-border transition on hover. |


---

[Table of contents](../index_en.md) - Forms | Previous: [Checkbox](02_checkbox_en.md) | Next: [ToggleSwitch](04_toggleswitch_en.md) | [Русский](03_radiobutton_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
