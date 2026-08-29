![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.9` · mod `0.2.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Forms | Previous: [InputText](07_inputtext_en.md) | Next: [Textarea](09_textarea_en.md) | [Русский](08_inputnumber_ru.md)

---

# InputNumber

A numeric field with +/− buttons, supporting integers or decimals, custom step size, and value
bounds.

`RimUI.Components.InputNumber : UiElement`

## Example

```csharp
new InputNumber(0, 100) { };
new InputNumber(-10, 10) { Integer = false, Step = 0.5 };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Min` | `double` | — | Lower bound. |
| `Max` | `double` | `100` | Upper bound. |
| `Step` | `double` | `1` | Increment used by the buttons. |
| `Integer` | `bool` | `true` | Integers only; `false` allows a decimal point or comma. |
| `OnChangeNum` | `Action<double>` | `null` | Called on commit and on +/− clicks. |
| `Initial` | `double` | `Min` | Initial buffer value. |
| `Disabled` | `bool` | `false` | Disabled state. |
| `ButtonsWidth` | `float` | `20` | Width of the +/− button column on the right. |

## Styles

Theme slots: `inputnumber/button`, `inputnumber/button_hover`, `inputnumber/button_disabled`, and
`inputnumber/button_icon`. The inner InputBase uses `input/frame` slots.

## Methods

| Method | Returns | Description |
|---|---|---|
| `InputNumber(double min, double max, Action<double> onChange = null)` (constructor) | — | Create a bounded numeric field with an optional callback. |
| `GetValue()` | `double` | Current buffer value; before first display, returns clamped `Initial`. |
| `SetValue(double v)` | — | Commit a value directly to the field buffer, bypassing `Filter` like the +/− buttons, and invoke `OnChangeNum`. |
| `SetStyle(string path, string value)` | — | The `input` part styles the inner InputBase frame, for example `SetStyle("input.borderColor", "#FF5050")`. The +/− buttons expose no separate parts. Unknown paths and values are silently ignored. |

### Exact character filtering

Every buffer edit filters the entire resulting string after the fact rather than blocking individual
key presses:

- Digits `0-9` are always allowed.
- A minus sign is allowed only when it would become the **first** character of the already filtered
  result and `Min < 0`. When `Min >= 0`, minus signs are always removed.
- A dot or comma is allowed only when `Integer == false` and no decimal point has appeared during
  this pass. Commas are normalized to dots in the buffer. In integer mode, all dots and commas are
  removed.
- All other characters, including letters, `+`, exponent `e`/`E`, and spaces, are silently dropped.

Commit parsing uses `double.TryParse(..., NumberStyles.Float, CultureInfo.InvariantCulture)`.
An empty or invalid string such as a lone `"-"` becomes 0 and is then clamped to `Min..Max`.
Integer formatting is `((long)Math.Round(v)).ToString()`; decimal formatting is
`v.ToString("0.##", InvariantCulture)`, with up to two fractional digits.

The +/− buttons bypass filtering: they parse the current buffer, add `±Step`, clamp, and write the
result directly to field state.

## Events

| Event | Type | Parameters | Fired when |
|---|---|---|---|
| `OnChangeNum` | `Action<double>` | new value | Commit by Enter/focus loss or a +/− click. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | InputNumber does not override `EffectiveDisabled`, so changing `Disabled` does not fire this event. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Slots for the buttons and inner input. |
| Sprite frame | Yes | Button backgrounds can use slot sprites. |
| Animation | Yes | Shared `Style.Animation` mechanism. |
| Disabled | Yes | Blocks the field and buttons. |
| Hover fade | No | Button hover switches `OverlayState` immediately. |


---

[Table of contents](../index_en.md) - Forms | Previous: [InputText](07_inputtext_en.md) | Next: [Textarea](09_textarea_en.md) | [Русский](08_inputnumber_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
