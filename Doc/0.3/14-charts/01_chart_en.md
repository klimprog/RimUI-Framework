![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.3.0` · RimWorld `1.6`

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
| `YMin` | `float?` | `null` | Lower bound of the value axis; `null` = 0. |
| `YMax` | `float?` | `null` | Upper bound; `null` = from the data. A value you set is **not** touched by rounding. |
| `YTickStep` | `float` | `0` | Step between ticks; `0` or less = pick it from the number of grid lines. |
| `NiceTicks` | `bool` | `true` | Round the picked step and top to "nice" ones — 1/2/5·10ⁿ. |
| `YFormat` | `Func<float,string>` | `null` | Your own format for axis labels and tooltips; `null` = the short format with no trailing zeros. |
| `static Palette` | `ColorRGBA[]` | 8 colors | Cycled when a series has no color. |

`ChartSeries` exposes `Name`, `Color`, `Values`, and `Provider`. Color alpha zero selects the palette. `Provider` takes priority over `Values` and runs every frame.

## The value axis

The axis settings live in the base `Chart`, so every kind has them at once: `LineChart`,
`ColumnChart`, `RadarChart`, `PolarChart`. In `HeatmapChart` the same `YMin`/`YMax` set the bounds
of the **colour** scale: without them each map is coloured from its own data, and the same colour
in two maps means different numbers.

**Nice ticks.** A label used to be computed as "maximum × k ÷ number of ticks", which is where
fractional "2.5 items" on whole-number data came from. With `NiceTicks` (on by default) the step
is rounded to 1/2/5·10ⁿ and the top is extended to a whole number of steps:

| Mode | Data 12, 19, 8, 15 with 4 grid lines |
|---|---|
| `NiceTicks = false` | 0, 4.75, 9.5, 14.25, 19 |
| `NiceTicks = true` | 0, 5, 10, 15, 20 |

```csharp
var c = new LineChart
{
    YMin = 20f, YMax = 60f, YTickStep = 10f,     // axis from 20 to 60, step 10
    YFormat = v => Chart.Fmt(v) + " u",          // units in the label
};
```

A `YMax` you set is never changed by rounding — you set it. Values outside the scale are pinned to
its edge, so a line, a column and a radar shape never spill past the grid. A column grows from the
**bottom of the scale**, not from zero: with `YMin` set, the bottom is shifted. The number of ticks
is capped at 64 — otherwise a step like `0.0001` over a wide range would flood the frame with
labels.

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
