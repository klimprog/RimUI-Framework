![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Charts | Previous: [PolarChart (Polar Area)](03_polarchart_en.md) | Next: [LineChart (Line / Area)](05_linechart_en.md) | [Русский](04_piechart_ru.md)

---

# PieChart (Pie / Donut)

One series divides a circle into category sectors. `InnerRadiusFraction` creates a donut.

`RimUI.Components.PieChart : Chart` (sealed)

## Example

```csharp
var pie = new PieChart { Style = { Height = 260f } };
pie.Labels.AddRange(new[] { "Wood", "Stone", "Metal", "Textiles" });
pie.Series.Add(new ChartSeries("", 35f, 25f, 20f, 20f));
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `InnerRadiusFraction` | `float` | `0` | Zero for pie; 0 to 0.9 for donut. |
| `ShowPercent` | `bool` | `true` | Shows percentages on sectors. |
| `MinPercentLabel` | `float` | `0.06` | Minimum fraction that receives a label. |

Only positive `Series[0]` values contribute to `total`; zero and negative values get no sector. Sector angle is `value / total * 360`, starting at 0 degrees at the top and proceeding clockwise in category order. Inner radius is clamped to `[0, 0.9]`.

No public methods or events beyond `Chart`.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style / sprite / animation | See `Chart` | Shared behavior. |
| Multiple series | No | Only `Series[0]`. |
| Tooltip | Yes | Sector hotspots. |
| Legend | Special | Categories rather than series. |


---

[Table of contents](../index_en.md) - Charts | Previous: [PolarChart (Polar Area)](03_polarchart_en.md) | Next: [LineChart (Line / Area)](05_linechart_en.md) | [Русский](04_piechart_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
