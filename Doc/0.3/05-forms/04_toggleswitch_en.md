![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.31` · mod `0.3.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Forms | Previous: [RadioButton](03_radiobutton_en.md) | Next: [ToggleButton](05_togglebutton_en.md) | [Русский](04_toggleswitch_ru.md)

---

# ToggleSwitch

A pill-shaped on/off switch whose thumb slides to the active side. Supports disabled state and
theme sprites.

`RimUI.Components.ToggleSwitch : UiElement`

## Example

```csharp
new ToggleSwitch(null, "Toggle") { Key = "sw1" };
new ToggleSwitch(null, "Disabled") { Disabled = true };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `LabelText` | `string` | — | Label text. |
| `LabelElement` | `Text` | `null` | Prepared label element. |
| `On` | `Func<bool>` | `null` | Value source. |
| `OnChange` | `Action<bool>` | `null` | Change callback. |
| `Disabled` | `bool` | `false` | Disabled state. |
| `TrackWidth` | `float` | `0` (theme, default 38) | Track width. |
| `TrackHeight` | `float` | `0` (theme, default 20) | Track height. |
| `Gap` | `float` | `0` (theme, default 8) | Gap between track and label. |
| `TrackStyle`, `TrackHoverStyle`, `TrackOnStyle`, `TrackDisabledStyle`, `KnobStyle`, `LabelStyle` | read-only `Style` | — | Explicit part styles. |

**The entire row is clickable**, including track, gap, and label. Hit testing uses the full
component `Bounds` with no extra expansion or shrinkage.

## Styles

Theme slots: `switch/track`, `switch/track_hover`, `switch/track_on`, `switch/knob`, and
`switch/label`. `switch/track_on`/`track_off` use 9-slice sprites; `knob` uses a regular sprite.

## Methods

| Method | Returns | Description |
|---|---|---|
| `ToggleSwitch(Action<bool> onChange = null, string label = null)` (constructor) | — | The callback comes **first**, followed by the label text. |
| `GetValue()` | `bool` | Current value from `On`, or the previous frame's internal cache. Before first display, returns `false` or a value supplied through `SetValue`. |
| `SetValue(bool v)` | — | Set the value as if clicked, updating internal state when no `On` source exists and invoking `OnChange`. Takes effect on the next frame and also works before first display. |
| `SetStyle(string path, string value)` | — | Parts: `track`, `track_hover`, `track_on`, `track_disabled`, `knob`, and `label`; for example `SetStyle("track.borderColor", "#FF5050")`. Unknown paths and values are silently ignored. |

## Events

| Event | Type | Parameters | Fired when |
|---|---|---|---|
| `OnChange` | `Action<bool>` | new value | A non-disabled switch is clicked. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | ToggleSwitch does not override `EffectiveDisabled`, so changing its `Disabled` field does not fire this event. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Track, knob, and label styles plus theme slots. |
| Sprite frame | Yes | 9-slice track and regular knob sprites. |
| Animation | Yes | Shared `Style.Animation`; the thumb also moves smoothly on its own, outside the animation registry. |
| Disabled | Yes | Uses `TrackDisabledStyle`. |
| Hover fade | Yes | Smooth track-border transition on hover. |


---

[Table of contents](../index_en.md) - Forms | Previous: [RadioButton](03_radiobutton_en.md) | Next: [ToggleButton](05_togglebutton_en.md) | [Русский](04_toggleswitch_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
