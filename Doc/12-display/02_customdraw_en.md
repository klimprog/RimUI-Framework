![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — core `0.6.7` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Display | Previous: [ImageBox](01_imagebox_en.md) | Next: [Gallery](03_gallery_en.md) | [Русский](02_customdraw_ru.md)

---

# CustomDraw

Custom rendering through portable framework commands in `OnDraw` or direct RimWorld `Widgets`/GUI calls in `OnDrawNative`.

`RimUI.Components.CustomDraw : UiElement`

## Example

```csharp
var chart = new CustomDraw {
    Style = { Width = 220f, Height = 90f },
    OnDraw = (list, rect, style, ctx) => {
        var bar = new RectF(rect.X, rect.Bottom - h, w, h);
        list.Add(DrawCommand.Background(bar, new Style { Background = Fill.Solid(Blue) }));
    }
};
var native = new CustomDraw {
    Style = { Width = 220f, Height = 60f },
    OnDrawNative = r => Widgets.Label(UnitConv.ToRect(r), "Text")
};
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `OnDraw` | `Action<DrawList, RectF, Style, LayoutContext>` | `null` | Portable framework-command rendering. |
| `OnDrawNative` | `Action<RectF>` | `null` | Raw window-coordinate rendering through `Widgets.*`. |
| `DefaultWidth` | `float` | `200` | Width when `Style.Width` is unset. |
| `DefaultHeight` | `float` | `120` | Height when `Style.Height` is unset. |

## Styles

Draw order is base background and border, `OnDraw`, then `OnDrawNative` on top.

## Methods

Only object-initializer construction is exposed.

| Method | Returns | Description |
|---|---|---|
| `SetStyle(string path, string value)` | - | Applies the path directly to the element style. |

## Events

`OnDraw` callbacks run every render frame rather than in response to user input. Universal events are also available:

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
| Style | Yes | Standard container styling. |
| Sprite frame | Indirectly | Base `Style.Sprite` works; no dedicated slot. |
| Animation | Yes, with caveat | Animated offset and scale affect native drawing, but opacity does not because raw `Widgets.*` bypasses adapter opacity. |
| Disabled | No | No disabled state. |
| Hover fade | Manual | Call `HoverFade.K(...)` inside `OnDraw`. |


---

[Table of contents](../index_en.md) - Display | Previous: [ImageBox](01_imagebox_en.md) | Next: [Gallery](03_gallery_en.md) | [Русский](02_customdraw_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
