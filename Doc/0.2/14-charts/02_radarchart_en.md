![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.9` · mod `0.2.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Charts | Previous: [Chart - shared chart base](01_chart_en.md) | Next: [PolarChart (Polar Area)](03_polarchart_en.md) | [Русский](02_radarchart_ru.md)

---

# RadarChart

A radar chart with one axis per `Label`, requiring at least three axes and sharing one scale across series.

`RimUI.Components.RadarChart : Chart` (sealed)

## Example

```csharp
var radar = new RadarChart { Style = { Height = 260f } };
radar.Labels.AddRange(new[] { "Speed", "Armor", "Damage", "Range", "Endurance" });
radar.Series.Add(new ChartSeries("Fighter A", 65f, 40f, 80f, 55f, 70f));
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `GridRings` | `int` | `4` | Grid polygons. |
| `FillAlpha` | `float` | `0.22` | Series fill alpha. |
| `StrokeWidth` | `float` | `2` | Outline width. |

Axes are spaced by `360 / n`. Axis 0 points up and indices advance clockwise. Radius is `min(area width, area height) * 0.5 - 16`. Ring `k` uses `radius * k / GridRings`. Nothing is drawn with fewer than three labels.

## Methods And Events

No public methods or events beyond `Chart`.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style / sprite / animation | See `Chart` | Shared behavior. |
| Live data | Yes | `ChartSeries.Provider` each frame. |
| Tooltip | Yes | Polygon vertices show category, series, and value. |
| Disabled / hover fade | No | Informational except tooltips. |


---

[Table of contents](../index_en.md) - Charts | Previous: [Chart - shared chart base](01_chart_en.md) | Next: [PolarChart (Polar Area)](03_polarchart_en.md) | [Русский](02_radarchart_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
