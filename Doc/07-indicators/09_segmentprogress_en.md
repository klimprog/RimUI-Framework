![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — core `0.6.7` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Indicators | Previous: [CircularProgress](08_circularprogress_en.md) | Next: [Divider](10_divider_en.md) | [Русский](09_segmentprogress_ru.md)

---

# SegmentProgress

A progress bar divided into whole segments, each with an optional color. Supports horizontal, vertical, and circular layouts.

`RimUI.Components.SegmentProgress : UiElement`

## Example

```csharp
var hSeg = new SegmentProgress(() => 0.6f) { Segments = 10 };
hSeg.SegmentColors.AddRange(rampColors);

var vSeg = new SegmentProgress(() => 0.6f) { Segments = 8, Vertical = true, Style = { Height = 90f } };
var cSeg = new SegmentProgress(() => 0.6f) { Segments = 12, Circular = true, Style = { Width = 72f } };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Value` | `Func<float>` | `null` | Value from 0 to 1. |
| `Segments` | `int` | `10` | Segment count. |
| `Gap` | `float` | `3` | Linear gap between segments. |
| `Vertical` | `bool` | `false` | Uses a vertical layout. |
| `Circular` | `bool` | `false` | Uses a circular layout. |
| `BarHeight` | `float` | `16` | Linear bar thickness. |
| `Thickness` | `float` | `8` | Circular ring thickness. |
| `GapDegrees` | `float` | `8` | Circular gap in degrees. |
| `SegmentColors` | `List<ColorRGBA>` | empty | Color by segment index; missing entries use the theme accent. |
| `ShowText` | `bool` | `true` | Shows value text. |
| `Format` | `Func<float,string>` | `null` | Formats value text. |

Filled count is `(int)(value * Segments + 0.5)`: `floor(x + 0.5)` rather than banker's rounding. For example, `0.55 * 10` fills 6 segments. At exactly `0.5 * 10 = 5.0`, truncation after adding `0.5` still yields 5.

In circular mode, `GapDegrees` is subtracted from each segment's equal angular slot. Each slot is `360 / Segments` and its painted arc is `segSpan = 360 / Segments - GapDegrees`, floored at 0.5 degrees. Unless that floor applies, arc plus gap across all slots totals 360 degrees.

## Styles

Size comes from `Style.Width` and `Style.Height`. Linear segments use `Fill.Solid` and circular ones use `ArcDraw.Arc`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `SegmentProgress(Func<float> value = null)` (constructor) | - | Creates an indicator with a value source. |
| `SetStyle(string path, string value)` | - | Applies a path such as `width` to the root style. |

## Events

No custom callbacks; universal events are available:

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered. Keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Size through `Style.Width` and `Style.Height`. |
| Sprite frame | No | Segments are color fills or procedural arcs. |
| Animation | Yes | Shared `Style.Animation`; no custom time animation. |
| Disabled | No | Non-interactive element. |
| Hover fade | No | No smooth hover transition. |


---

[Table of contents](../index_en.md) - Indicators | Previous: [CircularProgress](08_circularprogress_en.md) | Next: [Divider](10_divider_en.md) | [Русский](09_segmentprogress_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
