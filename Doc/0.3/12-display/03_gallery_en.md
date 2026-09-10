![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.31` · mod `0.3.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Display | Previous: [CustomDraw](02_customdraw_en.md) | Next: [AudioPlayer](../13-audio/01_audioplayer_en.md) | [Русский](03_gallery_ru.md)

---

# Gallery

An image gallery with a large frame, navigation arrows, thumbnail strip, and optional timed auto-advance.

`RimUI.Components.Gallery : UiElement`

## Example

```csharp
var g = new Gallery { Key = "gallery1", Style = { Width = 320f } };
g.Images.AddRange(new[] { "demo/photo1", "demo/photo2", "demo/photo3" });

var g2 = new Gallery { Key = "gallery2", AutoAdvance = 2.5f, ShowThumbnails = false,
    Style = { Width = 220f, Height = 140f } };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `Images` | `List<string>` | empty | Texture keys. |
| `AutoAdvance` | `float` | `0` | Seconds between automatic changes; zero disables. |
| `FrameHeight` | `float` | `200` | Main frame height. |
| `ThumbHeight` | `float` | `40` | Thumbnail height. |
| `ShowCounter` | `bool` | `true` | Shows current index out of total. |
| `ShowThumbnails` | `bool` | `true` | Shows thumbnails. |
| `OnChange` | `Action<int>` | `null` | Called for manual and automatic changes. |

Current index is stored by `Key`. Give simultaneous galleries unique keys.

### Exact `AutoAdvance` timing

State stores the absolute `LastAdvance` time. A change occurs when `ctx.Time - LastAdvance >= AutoAdvance`. Every actual manual or automatic transition updates `LastAdvance`, so manual navigation restarts the full interval. Clicking the already active thumbnail does not count and does not reset timing. The first interval starts on the first displayed frame. Image changes are immediate with no fade.

## Styles

Frames use fixed theme-palette colors rather than sprite slots.

## Methods

| Method | Returns | Description |
|---|---|---|
| `GetSelected()` | `int` | Current index, pending `SetSelected` value, or `0` before first render. |
| `SetSelected(int index)` | `void` | Changes on the next frame and raises normal callbacks. Index wraps with `((idx % n) + n) % n` rather than clamping. |
| `SetStyle(string path, string value)` | - | Applies to the root style; fixed frame and thumbnail styling is unaffected. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnChange` | `Action<int>` | new index | Manual, automatic, or programmed transition. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | Same transition; value is `Images[idx]`. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |

Universal clicks are independent of arrow and thumbnail handling.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `Style.Width` and `Style.Height`. |
| Sprite frame | No | Fixed palette styling. |
| Animation | Yes | Shared animation plus auto-advance timer. |
| Disabled | No | No disabled state. |
| Hover fade | Partly | Arrow background alpha switches immediately. |

There is no fullscreen view or zoom.


---

[Table of contents](../index_en.md) - Display | Previous: [CustomDraw](02_customdraw_en.md) | Next: [AudioPlayer](../13-audio/01_audioplayer_en.md) | [Русский](03_gallery_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
