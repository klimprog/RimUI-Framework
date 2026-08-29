![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Forms | Previous: [ToggleSwitch](04_toggleswitch_en.md) | Next: [Slider](06_slider_en.md) | [Русский](05_togglebutton_ru.md)

---

# ToggleButton

A button with separate captions for its two states, such as On and Off. Clicking toggles the value
and updates the caption. Internally, it renders a regular `Button`.

`RimUI.Components.ToggleButton : UiElement`

## Example

```csharp
new ToggleButton("On", "Off", null) { Key = "tb1" };
new ToggleButton("Pause", "Play", null);   // Arbitrary paired actions work too.
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `OnText` | `string` | — | Caption in the on state. |
| `OffText` | `string` | `OnText` when omitted | Caption in the off state. |
| `OnIcon` | `int` | `-1` | `Icons` index for the on state. |
| `OffIcon` | `int` | `-1` | `Icons` index for the off state. |
| `On` | `Func<bool>` | `null` | Value source. |
| `OnChange` | `Action<bool>` | `null` | Change callback. |
| `Disabled` | `bool` | `false` | Disabled state. |
| `OnPreset` | `ButtonPreset` (`Default`\|`Danger`\|`Warning`\|`Success`) | `Success` | Button preset for the on state; see Button for preset colors. |
| `OffPreset` | `ButtonPreset` | `Default` | Button preset for the off state. |

## Styles

Uses the regular Button style and theme slots. ToggleButton exposes no part styles of its own.

## Methods

| Method | Returns | Description |
|---|---|---|
| `ToggleButton(string onText, string offText = null, Action<bool> onChange = null)` (constructor) | — | When `offText` is omitted, both states use `onText`. |
| `GetValue()` | `bool` | Current value from `On`, or the previous frame's internal cache. Before first display, returns `false` or a value supplied through `SetValue`. |
| `SetValue(bool v)` | — | Set the value as if clicked, updating internal state when no `On` source exists and invoking `OnChange`. Takes effect on the next frame and works before first display. |
| `SetStyle(string path, string value)` | — | ToggleButton defines no parts and does not render its own root Style; it uses a private internal Button. This call has almost no visible effect. Use `OnPreset` and `OffPreset` for appearance. |

## Events

| Event | Type | Parameters | Fired when |
|---|---|---|---|
| `OnChange` | `Action<bool>` | new value | A non-disabled button is clicked. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | ToggleButton does not override `EffectiveDisabled`, so changing its `Disabled` field does not fire this event. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Same as the wrapped Button. |
| Sprite frame | Yes | Same as Button and its selected preset. |
| Animation | Yes | Shared `Style.Animation` mechanism. |
| Disabled | Yes | Blocks clicks. |
| Hover fade | Yes, inherited | Same smooth background transition as Button. |


---

[Table of contents](../index_en.md) - Forms | Previous: [ToggleSwitch](04_toggleswitch_en.md) | Next: [Slider](06_slider_en.md) | [Русский](05_togglebutton_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
