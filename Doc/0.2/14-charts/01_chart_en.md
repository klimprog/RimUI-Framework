![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Charts | Previous: [AudioPlayer](../13-audio/01_audioplayer_en.md) | Next: [RadarChart](02_radarchart_en.md) | [Русский](01_chart_ru.md)

---

# Chart - shared chart base

Abstract base for `RadarChart`, `PolarChart`, `PieChart`, `LineChart`, `ColumnChart`, and `HeatmapChart`. Provides series, legends, hover tooltips, automatic scale rounding, and flat JSON loading. Live data comes from `ChartSeries.Provider`.

`RimUI.Components.Chart : UiElement` (abstract)

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Series` | `List<ChartSeries>` | empty | Data series. |
| `Labels` | `List<string>` | empty | Category or axis labels. |
| `ShowLegend` | `bool` | `true` | Shows the legend. |
| `ShowTooltips` | `bool` | `true` | Shows hover tooltips. |
| `static Palette` | `ColorRGBA[]` | 8 colors | Cycled when a series has no color. |

`ChartSeries` exposes `Name`, `Color`, `Values`, and `Provider`. Color alpha zero selects the palette. `Provider` takes priority over `Values` and runs every frame.

## Exact `NiceMax` algorithm

```text
m = max / 10^floor(log10(max))
nice = 1 if m <= 1, 2 if m <= 2, 5 if m <= 5, otherwise 10
result = nice * 10^floor(log10(max))
```

Examples: 73 -> 100, 45 -> 50, 15 -> 20, and 6 -> 10.

## Methods

| Method | Returns | Description |
|---|---|---|
| `SeriesColor(int i)` | `ColorRGBA` | Explicit or palette color. |
| `LoadJson(string json, out string error)` | `bool` | Loads `labels` and `series` objects; false on error. |
| `static NiceMax(float max)` | `float` | Returns a 1/2/5 x 10^k upper bound. |
| `static Fmt(float v)` | `string` | Compact number formatting. |
| `SetStyle(string path, string value)` | - | Applies to the chart root. Legends and tooltips have no separate `Style`. |

## Events

No data callbacks; use `Provider`. Universal element events remain available. Hotspots distinguish tooltip targets but not click targets.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Standard chart background and border. |
| Sprite frame | Yes | Through `Style.Sprite`. |
| Animation | Yes | `Style.Animation`; live values use `Provider`. |
| Disabled | No | No disabled state. |
| Hover fade | No | Tooltips appear immediately. |


---

[Table of contents](../index_en.md) - Charts | Previous: [AudioPlayer](../13-audio/01_audioplayer_en.md) | Next: [RadarChart](02_radarchart_en.md) | [Русский](01_chart_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
