![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.9` · mod `0.2.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Indicators | Previous: [InlineMessage](04_inlinemessage_en.md) | Next: [ProgressBar](06_progressbar_en.md) | [Русский](05_metergroup_ru.md)

---

# MeterGroup

A bar split into proportional segments, with an optional legend below. Each segment's value, color, and label come from your data.

`RimUI.Components.MeterGroup : UiElement`

## Example

```csharp
var m = new MeterGroup().Add(55f, steelColor, "Steel").Add(25f, woodColor, "Wood")
                         .Add(10f, goldColor, "Gold");
m.Max = 100f;
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Segments` | `List<MeterSegment>` | empty | Bar segments. |
| `Max` | `float` | `0` (sum of segments) | Scale maximum. |
| `ShowLegend` | `bool` | `true` | Shows the legend below the bar. |
| `BarHeight` | `float` | `10` | Bar height. |
| `Format` | `Func<float,string>` | `null` | Formats values in the legend; default is the number. |

`MeterSegment(float value, ColorRGBA color, string label = null)` defines one segment.

## Styles

Track slot: `meter/track`, with sprite support. Legend slot: `meter/legend`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `MeterGroup()` (constructor) | - | Creates an empty group. |
| `MeterGroup(IEnumerable<MeterSegment> segments)` (constructor) | - | Creates a group from segments. |
| `Add(float value, ColorRGBA color, string label = null)` | `MeterGroup` | Adds a segment and returns this group for chaining. |
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
| Style | Partly | Resolved `Width` controls track width; radius comes from the slot. |
| Sprite frame | Yes (track) | `trackSlot.Sprite`. |
| Animation | Yes | Shared `Style.Animation` system. |
| Disabled | No | Non-interactive element. |
| Hover fade | No | No smooth hover transition. |


---

[Table of contents](../index_en.md) - Indicators | Previous: [InlineMessage](04_inlinemessage_en.md) | Next: [ProgressBar](06_progressbar_en.md) | [Русский](05_metergroup_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
