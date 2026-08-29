![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.9` · mod `0.2.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Forms | Previous: [InputNumber](08_inputnumber_en.md) | Next: [Field](10_field_en.md) | [Русский](09_textarea_ru.md)

---

# Textarea

A fixed-height multiline input for longer text. Its height is set as a number of visible rows.

`RimUI.Components.Textarea : InputBase`

## Example

```csharp
new Textarea(null) { Rows = 4, Placeholder = "Multiline text" };
```

## Parameters

`Textarea` adds only one property. Everything else is inherited from `InputBase`; the constructor also sets `Multiline = true`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Rows` | `int` | `4` | Height in text rows. Used only when `Style.Height` is not set explicitly. Formula: `Rows * 20f + 8f`. |

## Styles

The `input/frame` slot, including `_focus` and `_disabled`, is inherited from `InputBase`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Textarea(Action<string> onChange = null)` (constructor) | - | Creates a multiline input (`Multiline = true`). |

`Textarea` overrides only `Measure`, where it applies the height derived from `Rows` before calling the base implementation. Value and style methods are inherited unchanged from `InputBase`:

| Method | Returns | Description |
|---|---|---|
| `GetValue()` | `string` | Returns the current field text. If `Value` is set, it is used as the source; otherwise the method returns the value cached during the latest rendered frame. Before the first render, this is `""` unless a value was supplied through `SetValue`. |
| `SetValue(string v)` | - | Sets the text programmatically as if entered by the user. On the next frame, the value goes through the regular `Filter` -> `OnChange` pipeline. |
| `SetStyle(string path, string value)` | - | Style part: `frame`, for example `SetStyle("frame.borderColor", "#FF5050")`. Unknown paths and values are ignored. |

## Events

Inherited from `InputBase`: `OnChange` (`Action<string>`) and `OnCommit` (`Action<string>`). For multiline fields, `OnCommit` fires when focus is lost, not when Enter is pressed.

Universal events available on every element through `Events`:

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The pointer enters or leaves the element bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while the pointer is over the element. Keep the handler lightweight to avoid hurting FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | The mouse wheel is used over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | `Textarea` and `InputBase` do not override effective disabled state (`EffectiveDisabled`), so this event does not track the `Disabled` field and does not fire when that field changes. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Inherited `FrameStyle`. |
| Sprite frame | Yes | `input/frame` (9-slice). |
| Animation | Yes | Shared `Style.Animation` system. |
| Disabled | Yes | Inherited `Disabled` state. |
| Hover fade | No | Focus changes state immediately, without a smooth transition. |


---

[Table of contents](../index_en.md) - Forms | Previous: [InputNumber](08_inputnumber_en.md) | Next: [Field](10_field_en.md) | [Русский](09_textarea_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
