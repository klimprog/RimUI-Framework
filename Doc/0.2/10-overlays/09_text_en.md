![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.15` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [Icons](08_icons_en.md) | Next: [ConfirmPopup](10_confirmpopup_en.md) | [Русский](09_text_ru.md)

---

# Text

A text element.

`RimUI.Elements.Text : UiElement` (sealed)

## Example

```csharp
new Text("Panel A") { Style = { Text = new TextStyle { Align = TextAlign.Center,
    VAlign = VerticalAlign.Middle, Color = ColorRGBA.White } } };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Content` | `string` | constructor input | Text. |
| `Ts` | `TextStyle` | created on demand | Shorthand for `Style.Text ?? (Style.Text = new TextStyle())`. |

## Styles

Effective text style comes from the resolved `Theme.TextSlot` when a slot is set, otherwise from `Style.Text` or `Ts`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Text(string content)` (constructor) | - | Creates text. |
| `SetStyle(string path, string value)` | - | Applies root paths including `text.*`. |

Default width is the natural line width unless `Style.Width` is set. `TextStyle.Wrap` applies only when text exceeds available width.

## Events

No custom events.

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Never from `Text` itself. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Includes optional container background and border. |
| Sprite frame | Yes | Through base `UiElement`; padding and sprite frame affect `Measure`. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled | No | Non-interactive element. |
| Hover fade | No | No smooth hover transition. |


---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [Icons](08_icons_en.md) | Next: [ConfirmPopup](10_confirmpopup_en.md) | [Русский](09_text_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
