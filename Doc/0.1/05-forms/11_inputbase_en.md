![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Forms | Previous: [Field](10_field_en.md) | Next: [Select&lt;T&gt;](../06-selection/01_select_en.md) | [Русский](11_inputbase_ru.md)

---

# InputBase

The shared base class for `InputText`, `Textarea`, and the internal field used by `InputNumber`. It is not intended for direct use: unlike its descendants, it has no convenient factory-style constructors. It is useful when building a custom input with different rendering on top of the same text buffer and focus logic.

`RimUI.Elements.InputBase : UiElement` (not sealed)

## Example

The framework does not use `InputBase` directly in production code. Its shared properties are normally accessed through `InputText`, `Textarea`, or `InputNumber`:

```csharp
var field = new InputText();
field.Value = () => myText;
field.OnCommit = v => myText = v;
field.Placeholder = "...";
field.MaxLength = 40;
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Value` | `Func<string>` | `null` | Value source. It is read only while the field is not focused, so external state does not overwrite text while the user is typing. |
| `OnChange` | `Action<string>` | `null` | Called whenever the text changes. |
| `OnCommit` | `Action<string>` | `null` | Called on Enter for single-line fields, or when focus is lost. |
| `Filter` | `Func<string,string>` | `null` | Input filter used by `InputNumber` and the hex field in `ColorPicker`. |
| `Placeholder` | `string` | `null` | Hint shown while the field is empty. |
| `ReadOnly` | `bool` | `false` | Prevents editing. |
| `Disabled` | `bool` | `false` | Disables the field. |
| `MaxLength` | `int` | `0` (unlimited) | Maximum text length. |
| `Multiline` | `bool` | `false` | Enables multiline input. |
| `MinHeight` | `float` | `28` | Height of a single-line field. |
| `FrameStyle` | `Style` (readonly) | - | Explicit frame style that overrides the `input/frame`, `input/frame_focus`, and `input/frame_disabled` theme slots. |

## Styles

Theme slots: `input/frame`, `input/frame_focus`, and `input/frame_disabled`. All three support a 9-slice sprite frame.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Focus(LayoutContext ctx)` | `void` | Requests focus programmatically for the next frame. |
| `StateOf(UiState s)` | `InputState` | Returns the internal text-buffer and focus state. Used by compound controls such as `InputNumber` and the hex field in `ColorPicker`. |
| `GetValue()` | `string` | Returns the current field text. If `Value` is set, it is used as the source; otherwise the method returns the value cached during the latest rendered frame. Before the first render, this is `""` unless a value was supplied through `SetValue`. |
| `SetValue(string v)` | - | Sets text programmatically as if entered by the user. On the next frame, the value goes through `Filter` -> `OnChange`. When `Value` is set, it remains authoritative: update its backing state in `OnChange`, or it will restore the previous value. |
| `SetStyle(string path, string value)` | - | Style part: `frame`, for example `SetStyle("frame.borderColor", "#FF5050")`. Unknown paths and values are ignored. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnChange` | `Action<string>` | new text | Whenever the field contents change. |
| `OnCommit` | `Action<string>` | committed text | On Enter for single-line fields, or when focus is lost. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The pointer enters or leaves the element bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while the pointer is over the element. Keep the handler lightweight to avoid hurting FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | The mouse wheel is used over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | `InputBase` does not override effective disabled state (`EffectiveDisabled`), so this event does not track the `Disabled` field and does not fire when that field changes. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `FrameStyle`. |
| Sprite frame | Yes | `input/frame` (9-slice). |
| Animation | Yes | Shared `Style.Animation` system. |
| Disabled | Yes | Blocks editing. |
| Hover fade | No | Focus changes `OverlayState` immediately, without a smooth transition. |


---

[Table of contents](../index_en.md) - Forms | Previous: [Field](10_field_en.md) | Next: [Select&lt;T&gt;](../06-selection/01_select_en.md) | [Русский](11_inputbase_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
