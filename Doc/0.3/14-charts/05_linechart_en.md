![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.3.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Charts | Previous: [PieChart (Pie / Donut)](04_piechart_en.md) | Next: [ColumnChart (Column / Bar)](06_columnchart_en.md) | [Русский](05_linechart_ru.md)

---

# LineChart (Line / Area)

Draws one or more series as polylines. Area mode fills from each curve down to the zero line.

`RimUI.Components.LineChart : Chart` (sealed)

## Example

```csharp
var line = new LineChart { Style = { Height = 240f } };
line.Labels.AddRange(months);
line.Series.Add(new ChartSeries("Mining", 20f, 35f, 30f, 50f, 45f, 60f));
var area = new LineChart { Style = { Height = 240f }, Area = true };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Area` | `bool` | `false` | Fills under curves when true. |
| `FillAlpha` | `float` | `0.22` | Area fill alpha. |
| `StrokeWidth` | `float` | `2` | Line width. |
| `ShowMarkers` | `bool` | `true` | Draws point markers. |
| `MarkerRadius` | `float` | `3.5` | Marker radius. |
| `ShowGrid` | `bool` | `true` | Shows grid. |
| `GridLines` | `int` | `4` | Grid line count. |

No public methods or events beyond `Chart`.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style / sprite / animation | See `Chart` | Shared behavior. |
| Multiple series | Yes | Several polylines. |
| Tooltip | Yes | Point hotspots remain active even when markers are hidden. |


---

[Table of contents](../index_en.md) - Charts | Previous: [PieChart (Pie / Donut)](04_piechart_en.md) | Next: [ColumnChart (Column / Bar)](06_columnchart_en.md) | [Русский](05_linechart_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
