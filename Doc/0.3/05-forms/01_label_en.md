![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.3.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Forms | Previous: [StyleAnimation](../04-animations/03_styleanimation_en.md) | Next: [Checkbox](02_checkbox_en.md) | [Русский](01_label_ru.md)

---

# Label

A form-field label with an optional leading icon, required marker, and hint line below it.

`RimUI.Components.Label : FlexBox`

## Example

```csharp
new Label("Colonist name");
new Label("Password") { Required = true, Hint = "At least 8 characters", IconIndex = Icons.Info };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Content` | `string` | `null` | Raw label text. |
| `TextElement` | `Text` | `null` | Prepared text element; takes priority over `Content`. |
| `Required` | `bool` | `false` | Draw an asterisk after the text. |
| `Hint` | `string` | `null` | Raw hint text below the label. |
| `HintElement` | `Text` | `null` | Prepared hint element; takes priority over `Hint`. |
| `IconElement` | `Icon` | `null` | Prepared icon before the text. |
| `IconIndex` | `int` | `-1` | Index in the built-in `Icons` sheet; alternative to `IconElement`. |
| `For` | `UiElement` | `null` | Reserved for focusing a field by clicking its label; not implemented yet. |

## Styles

Inherits `FlexBox.Style` for group background, border, and gap. Style the label and hint text
through their own `TextElement.Style.Text` and `HintElement.Style.Text`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Label(string content = null)` (constructor) | — | Create a text label. |
| `SetStyle(string path, string value)` | — | Label defines no `StylePart`; the path applies directly to inherited root Style, for example `SetStyle("text.color", "#FF0000")`. Unknown paths and invalid values are silently ignored. |

Label stores no value and has no `GetValue`/`SetValue` methods.

## Events

No component-specific callbacks. Universal events available through `Events`:

| Event | Type | Parameters | Fired when |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered. Keep this handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Never fires because Label has no disabled state. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Same as FlexBox. |
| Sprite frame | Yes | Inherited through `Style.Sprite`. |
| Animation | Yes | Shared `Style.Animation` mechanism. |
| Disabled | No | Label has no disabled state. |
| Hover fade | No | Label is not interactive. |


---

[Table of contents](../index_en.md) - Forms | Previous: [StyleAnimation](../04-animations/03_styleanimation_en.md) | Next: [Checkbox](02_checkbox_en.md) | [Русский](01_label_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
