![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — core `0.6.7` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Forms | Previous: [Textarea](09_textarea_en.md) | Next: [InputBase](11_inputbase_en.md) | [Русский](10_field_ru.md)

---

# Field

A styled flex group: a bordered card or container for arbitrary content. Its background, border, radius, padding, and gap defaults come from `Theme.Field`.

`RimUI.Elements.Field : FlexBox`

## Example

```csharp
var f = new Field();
f.Style.Width = 240f;
f.Style.Height = 90f;
f.Style.Sprite = new SpriteFrame("demo/pattern", 3f) { Repeat = true };
```

## Parameters

`Field` has no properties of its own. It exposes the full `FlexBox` API, including `Axis`, `Grow`, and `DragGroup`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| - | - | - | No field-specific parameters; see `FlexBox`. |

## Styles

The default theme slot is `Theme.Field`: `Padding = 12`, `Gap = 8`, `Radius = Small`, `BorderWidth = Small`, `BorderColor` with `0.35` alpha, and `Background = SurfaceAlt`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Field()` (constructor) | - | Creates a field with the `Column` axis. |
| `Field(Axis axis)` (constructor) | - | Creates a field with an explicitly selected axis. |
| `SetStyle(string path, string value)` | - | `Field` does not define style parts (`StylePart`), so the path applies directly to its root `Style`, for example `SetStyle("background", "#2A2A2A")`. Unknown paths and values are ignored. |

All other methods are inherited from `FlexBox`, including `Add`, `RemoveChildAt`, and `InsertChild`. `Field` does not hold a value and therefore has no `GetValue` or `SetValue` methods.

## Events

`Field` has no custom events. See `FlexBox` for drag-and-drop callbacks available when `DragGroup` is set. Universal events available through `Events`:

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The pointer enters or leaves the element bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while the pointer is over the element. Keep the handler lightweight to avoid hurting FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click on the element. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | The mouse wheel is used over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | `Field` has no disabled state, so this event never fires. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Defaults come from `Theme.Field`. |
| Sprite frame | Yes | Through `Style.Sprite`. |
| Animation | Yes | Shared `Style.Animation` system. |
| Disabled | No | The element has no disabled state of its own. |
| Hover fade | No | `Field` is not interactive by itself. |
| Drag and drop | Yes | Inherited from `FlexBox` as a list-style drop zone. |


---

[Table of contents](../index_en.md) - Forms | Previous: [Textarea](09_textarea_en.md) | Next: [InputBase](11_inputbase_en.md) | [Русский](10_field_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
