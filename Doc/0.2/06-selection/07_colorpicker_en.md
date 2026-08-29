![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.9` · mod `0.2.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Selection | Previous: [CascadeSelect](06_cascadeselect_en.md) | Next: [Tag](../07-indicators/01_tag_en.md) | [Русский](07_colorpicker_ru.md)

---

# ColorPicker

A color selector with an HSB hue/saturation/brightness square, a hue strip, an alpha slider, and editable hex input. It does not include an eyedropper.

`RimUI.Components.ColorPicker : UiElement`

## Example

```csharp
new ColorPicker(c => _picked = c) { Value = () => _picked };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Value` | `Func<ColorRGBA>` | `null` | Source of the current color. |
| `OnChange` | `Action<ColorRGBA>` | `null` | Change callback. |
| `Disabled` | `bool` | `false` | Disables the control. |
| `ShowHex` | `bool` | `true` | Shows editable hex input below the palette. |
| `ShowAlpha` | `bool` | `true` | Shows the alpha strip and includes alpha in hex input. |
| `FieldWidth` | `float` | `44` | Color swatch field width. |
| `FieldHeight` | `float` | `24` | Color swatch field height. |
| `SquareW` | `float` | `160` | HSB square width. |
| `SquareH` | `float` | `120` | HSB square height. |
| `HueW` | `float` | `16` | Hue and alpha strip width. |

## Styles

Theme slots: `colorpicker/field` for the swatch border and `colorpicker/panel` for the sprite-capable panel. The HSB square and strips use custom gradients rather than sprites.

## Methods

| Method | Returns | Description |
|---|---|---|
| `ColorPicker(Action<ColorRGBA> onChange = null)` (constructor) | - | Creates a color picker with a callback. |
| `GetValue()` | `ColorRGBA` | Returns `Value` when supplied, otherwise the latest cached color or the value from `SetValue`. This component uses `GetValue`/`SetValue` even though it also raises `Events.SelectionChanged`. |
| `SetValue(ColorRGBA v)` | - | Sets the color programmatically. Without `Value`, updates internal HSB state on the next frame; always calls `OnChange`. |
| `SetStyle(string path, string value)` | - | Applies a theme-style path such as `"colorpicker.field.bordercolor"` to the element itself. Unknown paths and invalid values are ignored. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnChange` | `Action<ColorRGBA>` | new color | The HSB square or a strip is dragged, or valid hex input is committed. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The pointer enters or leaves the element bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered. Keep the handler lightweight and avoid allocations, searches, or I/O. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | The mouse wheel is used over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | The color changes after `OnChange`. Index is always `-1`; value is the new boxed `ColorRGBA`. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `colorpicker/field` and `colorpicker/panel`. |
| Sprite frame | Yes (panel only) | `colorpicker/panel` may use a sprite. |
| Animation | Yes | Shared `Style.Animation` system. |
| Disabled | Yes | Dims the swatch and frame and prevents the panel from opening. |
| Hover fade | No | No smooth hover transition. |


---

[Table of contents](../index_en.md) - Selection | Previous: [CascadeSelect](06_cascadeselect_en.md) | Next: [Tag](../07-indicators/01_tag_en.md) | [Русский](07_colorpicker_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
