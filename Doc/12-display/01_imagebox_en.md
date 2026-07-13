![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — core `0.6.7` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Display | Previous: [ConfirmWindow](../11-windows/04_confirmwindow_en.md) | Next: [CustomDraw](02_customdraw_en.md) | [Русский](01_imagebox_ru.md)

---

# ImageBox

Displays a texture with normal spacing, radius, and border styles. Supports stretch, contain, cover, and natural-size fitting. The class is named `ImageBox` because `Image` is used by `Fill.Image(...)`.

`RimUI.Components.ImageBox : UiElement`

## Example

```csharp
var framed = new ImageBox("demo/pattern") {
    Fit = ImageFit.Cover,
    Style = { Width = 96f, Height = 96f, Radius = BorderRadius.Middle,
              BorderWidth = BorderWidth.Middle, BorderColor = Orange }
};
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `TextureKey` | `string` | `null` | Texture key. |
| `Fit` | `ImageFit` | `Contain` | `Stretch`, `Cover`, `Contain`, or `Auto`. |
| `Tint` | `ColorRGBA` | `White` | Image tint. |

## Exact fitting math

`Contain` uses `k = min(area.width / tex.width, area.height / tex.height)` and positions the full scaled image by `Fill.ImageAnchor`, default `(0.5, 0.5)`.

`Cover` uses `k = max(...)`. It computes an anchored visible texture crop that covers the area. `Auto` uses 1:1 size per axis; smaller textures are anchored inside the area and larger ones are cropped without scaling.

When `Style.Radius != None`, native masked rendering is used and `ImageAnchor` is ignored. `Cover` and `Auto` then crop from the center.

## Styles

Implemented as `Style.Background = Fill.Image(...)`, so margin, padding, radius, border, and shadow work normally.

## Methods

| Method | Returns | Description |
|---|---|---|
| `ImageBox(string textureKey = null)` | - | Creates an image. |
| `SetStyle(string path, string value)` | - | Applies the path directly to the element style. |

## Events

No custom events.

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Full element styling through `Style.Background`. |
| Sprite frame | Indirectly | Base `Style.Sprite` works, but there is no image-specific sprite slot. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled | No | Non-interactive element. |
| Hover fade | No | No smooth hover transition. |


---

[Table of contents](../index_en.md) - Display | Previous: [ConfirmWindow](../11-windows/04_confirmwindow_en.md) | Next: [CustomDraw](02_customdraw_en.md) | [Русский](01_imagebox_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
