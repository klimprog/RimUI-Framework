![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — core `0.6.7` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Indicators | Previous: [ProgressSpinner](07_progressspinner_en.md) | Next: [SegmentProgress](09_segmentprogress_en.md) | [Русский](08_circularprogress_ru.md)

---

# CircularProgress

A circular progress indicator with full, half, and three-quarter arcs; configurable start and direction; an arc gradient and borders; and optional center text or icon.

`RimUI.Components.CircularProgress : UiElement`

## Example

```csharp
var c = new CircularProgress(() => 0.7f) { Style = { Width = 72f } };
c.BarColorEnd = Orange; c.OuterBorder = 1f; c.InnerBorder = 1f;

var icon = new CircularProgress(() => 1f) { Icon = Icons.Check, Style = { Width = 72f } };
var half = new CircularProgress(() => 0.7f) { Shape = ArcShape.Half, Style = { Width = 72f } };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Value` | `Func<float>` | `null` | Value from 0 to 1. |
| `Shape` | `ArcShape` (`Full`, `Half`, `ThreeQuarter`) | `Full` | Arc shape. |
| `Start` | `ArcStart` (`Top`, `Right`, `Bottom`, `Left`) | `Top` | Start point, used only by `Full`. |
| `Reverse` | `bool` | `false` | Fills counterclockwise. |
| `Thickness` | `float` | `6` | Bar thickness. |
| `OuterBorder` | `float` | `0` | Outer border thickness. |
| `InnerBorder` | `float` | `0` | Inner border thickness. |
| `BarColor` | `ColorRGBA?` | `null` | Bar color and gradient start. |
| `BarColorEnd` | `ColorRGBA?` | `null` | Gradient end along the arc. |
| `TrackColor` | `ColorRGBA?` | `null` | Background track color. |
| `BorderColor` | `ColorRGBA?` | `null` | Border color. |
| `ShowText` | `bool` | `true` | Shows the value in the center. |
| `Format` | `Func<float,string>` | `null` | Formats center text. |
| `Icon` | `int` | `-1` | Center `Icons` index; takes priority over text. |
| `IconSize` | `float` | `16` | Center icon size. |

## Exact arc geometry

Angles use a clock-face coordinate system: **0 degrees is the top, increasing clockwise**.

For `Shape = Full`, `Start` maps to `Top = 0`, `Right = 90`, `Bottom = 180`, and `Left = 270` degrees. `Start` is ignored by the other shapes.

| `ArcShape` | Start | Span | Visual gap |
|---|---|---|---|
| `Full` | selected `Start` | 360 degrees | none |
| `Half` | 270 degrees (left) | 180 degrees | lower half |
| `ThreeQuarter` | 225 degrees (lower left) | 270 degrees | 90-degree gap at the bottom, from 135 to 225 degrees |

Normally the filled arc grows clockwise from the range start by `span * value`. With `Reverse = true`, it starts at the range end (`start + span`) and grows counterclockwise toward the start. This fills from the opposite edge rather than merely reversing the gradient in place.

## Styles

Size comes from `Style.Width` and `Style.Height`. Center text uses `circular/text`. Arcs are drawn procedurally by `ArcDraw.Arc`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `CircularProgress(Func<float> value = null)` (constructor) | - | Creates an indicator with a value source. |
| `SetStyle(string path, string value)` | - | Applies a path such as `width` to the root style. Center text is customized through `TextStyleOverride`. |

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
| Sprite frame | No | Arc and borders are procedural color fills. |
| Animation | Yes | Shared `Style.Animation`; the arc itself does not rotate. |
| Disabled | No | Non-interactive element. |
| Hover fade | No | No smooth hover transition. |


---

[Table of contents](../index_en.md) - Indicators | Previous: [ProgressSpinner](07_progressspinner_en.md) | Next: [SegmentProgress](09_segmentprogress_en.md) | [Русский](08_circularprogress_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
