![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.3.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Forms | Previous: [Field](10_field_en.md) | Next: [Rating](12_rating_en.md) | [Русский](11_inputbase_ru.md)

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
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
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
| `GetText()` | `string` | The field's current text, taken from its state — it works even when the field was **not drawn** this frame (a hidden tab, a collapsed panel). Before the first frame it returns `""`. |
| `Flush()` | `bool` | Commit what was typed without waiting for Enter or for focus to be lost: the player types a value and presses "Save" straight away. Calls `OnChange` (if the text changed) and `OnCommit`. `false` = the field has never taken part in a frame. |
| `SetStyle(string path, string value)` | - | Style part: `frame`, for example `SetStyle("frame.borderColor", "#FF5050")`. Unknown paths and values are ignored. |

## The value while the field is off-screen

A field's state lives in the ID store and survives frames in which the field was not drawn. That
is why `GetText()` (and `GetValue()` on `InputNumber`) returns the current value even from a
hidden tab or a collapsed panel.

The same fact makes an explicit key **mandatory** in one case: the state is found by `Key`, so a
field that gets recreated (a new object every frame, or every time the window opens) must have an
explicit `Key` — otherwise the automatic key changes along with the object and the value is lost.

```csharp
var name = new InputText { Key = "colony.name" };

var save = Button.Make("Save");
save.OnClick = () =>
{
    name.Flush();                 // commit what was typed: focus was never lost, Enter never pressed
    Settings.ColonyName = name.GetText();
};
```

The difference between the callbacks: `OnChange` fires on **every** change to the text, `OnCommit`
fires on Enter and on losing focus. `Flush()` fires both, as if the player had pressed Enter.

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

[Table of contents](../index_en.md) - Forms | Previous: [Field](10_field_en.md) | Next: [Rating](12_rating_en.md) | [Русский](11_inputbase_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
