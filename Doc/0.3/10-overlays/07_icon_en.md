![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.3.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [TooltipBox](06_tooltipbox_en.md) | Next: [Icons](08_icons_en.md) | [Русский](07_icon_ru.md)

---

# Icon

A tinted and optionally rotated image from a texture or atlas.

`RimUI.Elements.Icon : UiElement` (sealed)

## Example

```csharp
var raw = new Icon("MyMod/MyIcon") { Tint = ColorRGBA.White, Fit = 0.9f };
var rotated = Icons.Get(Icons.ChevronRight, 20f);
rotated.Rotation = 45f;
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `IconKey` | `string` | constructor input | Texture or atlas key. |
| `Tint` | `ColorRGBA` | `White` | Image tint. |
| `Fit` | `float` | `1` | Fraction of the content area occupied by the image. |
| `Rotation` | `float` | `0` | Clockwise rotation around center in degrees. |
| `Source` | `RectF?` | `null` | Atlas subrectangle; null uses the whole texture. |

## Styles

Inherited container background, border, radius, and padding are supported. `ContentRect` determines icon placement.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Icon(string iconKey)` (constructor) | - | Creates an icon for a texture key. |
| `SetStyle(string path, string value)` | - | Applies the path to the root style. |

## Events

No custom events.

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Never from `Icon` itself. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Container background, border, and radius. |
| Sprite frame | Yes | Base `Style.Sprite` for the container background. |
| Animation | Yes | Shared `Style.Animation`; `Rotation` itself is a static angle. |
| Disabled | No | Non-interactive element. |
| Hover fade | No | No smooth hover transition. |


---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [TooltipBox](06_tooltipbox_en.md) | Next: [Icons](08_icons_en.md) | [Русский](07_icon_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
