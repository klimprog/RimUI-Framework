![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Charts | Previous: [ColumnChart (Column / Bar)](06_columnchart_en.md) | Next: [NodeCanvas](../15-nodecanvas/01_nodecanvas_en.md) | [Русский](07_heatmapchart_ru.md)

---

# HeatmapChart

A series-by-category matrix. Rows are series, columns are `Labels`, and every cell color is interpolated against the global minimum and maximum of the entire matrix.

`RimUI.Components.HeatmapChart : Chart` (sealed)

## Example

```csharp
var hm = new HeatmapChart { Style = { Height = 220f } };
hm.Labels.AddRange(days);
hm.Series.Add(new ChartSeries("Colonist A", 2f, 5f, 3f, 6f, 1f));
hm.Series.Add(new ChartSeries("Colonist B", 4f, 2f, 6f, 3f, 5f));
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `MinColor` | `ColorRGBA` | blue | Global minimum color. |
| `MaxColor` | `ColorRGBA` | red | Global maximum color. |
| `ShowValues` | `bool` | `true` | Prints values with contrast-aware text. |

## Exact color interpolation

```text
t = clamp01((value - globalMin) / (globalMax - globalMin))
color = MinColor + (MaxColor - MinColor) * t
```

Interpolation is linear across RGBA and uses the whole matrix. If all values are equal, every cell uses exactly `MinColor`. Text switches between white and dark around luminance 0.6 using `0.299R + 0.587G + 0.114B`.

Legend is intentionally disabled because the scale is continuous. No public methods or events beyond `Chart`.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style / sprite / animation | See `Chart` | Shared behavior. |
| Legend | No | Intentionally disabled. |
| Tooltip | Yes | Every cell has a hotspot. |


---

[Table of contents](../index_en.md) - Charts | Previous: [ColumnChart (Column / Bar)](06_columnchart_en.md) | Next: [NodeCanvas](../15-nodecanvas/01_nodecanvas_en.md) | [Русский](07_heatmapchart_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
