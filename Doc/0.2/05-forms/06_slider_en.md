![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Forms | Previous: [ToggleButton](05_togglebutton_en.md) | Next: [InputText](07_inputtext_en.md) | [Русский](06_slider_ru.md)

---

# Slider

Drag a thumb to change a value. Slider supports steps, units, tick marks, end labels, a two-thumb
range mode, vertical orientation, and disabled state.

`RimUI.Components.Slider : UiElement`

## Example

```csharp
new Slider(0f, 100f) { Step = 5f, Unit = "%", ShowTicks = true, ShowEndLabels = true };
new Slider(0f, 20f) { Range = true, Step = 1f, ShowEndLabels = true };   // Two-thumb range.
new Slider(0f, 100f) { Vertical = true, Style = { Height = 120f } };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `Min` | `float` | — | Lower bound. |
| `Max` | `float` | `100` | Upper bound. |
| `Step` | `float` | `1` | Value step. |
| `Value` | `Func<float>` | `null` | Value source for single mode or the lower thumb in range mode. |
| `OnChange` | `Action<float>` | `null` | Callback for the single/lower thumb. |
| `Range` | `bool` | `false` | Enable two-thumb range mode. |
| `ValueHi` | `Func<float>` | `null` | Upper-thumb value source in range mode. |
| `OnChangeHi` | `Action<float>` | `null` | Upper-thumb callback. |
| `Disabled` | `bool` | `false` | Disabled state. |
| `ShowValue` | `bool` | `true` | Show the value above the thumb. |
| `ShowTicks` | `bool` | `false` | Show tick marks below the track. |
| `ShowEndLabels` | `bool` | `false` | Show min/max labels at the ends. |
| `Vertical` | `bool` | `false` | Vertical orientation with min at the bottom. |
| `Unit` | `string` | `""` | Unit appended to the value label. |
| `Format` | `Func<float,string>` | `null` | Custom value formatter. |
| `TrackHeight` | `float` | `0` (theme `slider/track`, default 8) | Track thickness. |
| `HandleSize` | `float` | `0` (theme `slider/handle`, default 16) | Thumb size. |
| `TrackStyle`, `FillStyle`, `HandleStyle`, `ValueStyle`, `EndsStyle`, `TickStyle`, `TickMajorStyle` | read-only `Style` | — | Explicit part styles. |

## Exact drag behavior

The hit area covers the **entire track at thumb thickness** (`_hs`), not just the thumb. Clicking
anywhere on the track immediately moves the value under the pointer. With
`i = HandleSize/2`, which keeps the thumb inside the track ends, pointer position maps to 0..1 as:

```
Horizontal: f = (mouseX - (track.X + i)) / (track.Width  - 2i)
Vertical:   f = (track.Bottom - i - mouseY) / (track.Height - 2i)
f = clamp(f, 0, 1)
```

Step rounding uses `Math.Round` on the offset from `Min`, then clamps again:

```
value = clamp(Min + Round((Min + f*(Max-Min) - Min) / Step) * Step, Min, Max)
```

In range mode, the thumb closest to the pointer on X is selected:
`|mouseX - xLow| < |mouseX - xHigh|` selects the upper thumb. There is **no minimum gap** between
thumbs. Crossing is prevented only by strict `nv < lo` for the upper thumb and `nv > hi` for the
lower one, so `lo == hi` is valid and the thumbs may meet exactly.

## Styles

Theme slots: `slider/track` (9-slice), `slider/handle`, `slider/handle_active`, and
`slider/handle_disabled`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Slider(float min, float max, Action<float> onChange = null)` (constructor) | — | Create a bounded slider with an optional callback. |
| `GetValue()` | `float` | Current single/lower value from `Value`, or the previous frame's internal cache. |
| `SetValue(float v)` | — | Set the single/lower value, updating internal state when no `Value` source exists and invoking `OnChange` on the next frame. |
| `GetValueHi()` | `float` | Current upper value in range mode, from `ValueHi` or the previous frame's cache. |
| `SetValueHi(float v)` | — | Set the upper value in range mode, updating internal state when no `ValueHi` source exists and invoking `OnChangeHi`. |
| `SetStyle(string path, string value)` | — | Parts: `track`, `fill`, `handle`, `value`, `ends`, `tick`, and `tick_major`; for example `SetStyle("handle.borderColor", "#FF5050")`. Unknown paths and values are silently ignored. |

## Events

| Event | Type | Parameters | Fired when |
|---|---|---|---|
| `OnChange` | `Action<float>` | new value | The single/lower thumb is dragged. |
| `OnChangeHi` | `Action<float>` | new value | The upper thumb is dragged in range mode. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Slider does not override `EffectiveDisabled`, so changing its `Disabled` field does not fire this event. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | All part styles plus theme slots. |
| Sprite frame | Yes | 9-slice track and regular handle sprite. |
| Animation | Yes | Shared `Style.Animation` mechanism. |
| Disabled | Yes | Uses `handle_disabled` and blocks dragging. |
| Hover fade | No | Handle state switches immediately between active and disabled slots. |


---

[Table of contents](../index_en.md) - Forms | Previous: [ToggleButton](05_togglebutton_en.md) | Next: [InputText](07_inputtext_en.md) | [Русский](06_slider_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
