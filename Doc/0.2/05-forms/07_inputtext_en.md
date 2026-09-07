![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Forms | Previous: [Slider](06_slider_en.md) | Next: [InputNumber](08_inputnumber_en.md) | [Русский](07_inputtext_ru.md)

---

# InputText

A single-line text field with optional placeholder, bound value, icons, read-only or disabled state,
and a custom frame sprite.

`RimUI.Components.InputText : InputBase`

## Example

```csharp
new InputText { Placeholder = "Enter a name" };
new InputText { Placeholder = "Search", LeftIcon = Icons.Info };
new InputText { Value = () => "read-only", ReadOnly = true };
```

## Parameters

Fields specific to InputText; see InputBase for inherited fields:

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `LeftIcon` | `int` | `-1` | `Icons` index drawn inside the left side of the frame; reduces content width. |
| `RightIcon` | `int` | `-1` | `Icons` index drawn inside the right side of the frame. |

## Styles

Inherits the `input/frame`, `_focus`, and `_disabled` slots from InputBase, including 9-slice
frame sprites.

## Methods

| Method | Returns | Description |
|---|---|---|
| `InputText(Action<string> onChange = null)` (constructor) | — | Create a field with an optional change callback. |

Only `Emit` is overridden to draw icons over the InputBase field. Value and style methods are
inherited unchanged:

| Method | Returns | Description |
|---|---|---|
| `GetValue()` | `string` | Current text from `Value`, or the previous frame's cache. Before first display, returns `""` or a value supplied through `SetValue`. |
| `SetValue(string v)` | — | Set text as user input; on the next frame it passes through the normal `Filter` → `OnChange` pipeline. |
| `SetStyle(string path, string value)` | — | Supports the `frame` part, for example `SetStyle("frame.borderColor", "#FF5050")`. Unknown paths and values are silently ignored. |

## Events

Inherited from InputBase: `OnChange` (`Action<string>`) on every edit and `OnCommit`
(`Action<string>`) on Enter or focus loss.

Universal events:

| Event | Type | Parameters | Fired when |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | InputBase does not override `EffectiveDisabled`, so changing `Disabled` does not fire this event. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Inherited `FrameStyle`. |
| Sprite frame | Yes | 9-slice `input/frame`. |
| Animation | Yes | Shared `Style.Animation` mechanism. |
| Disabled | Yes | Inherited `Disabled`. |
| Hover fade | No | Focus switches `OverlayState` immediately. |


---

[Table of contents](../index_en.md) - Forms | Previous: [Slider](06_slider_en.md) | Next: [InputNumber](08_inputnumber_en.md) | [Русский](07_inputtext_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
