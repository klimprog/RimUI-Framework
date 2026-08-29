![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Charts | Previous: [LineChart (Line / Area)](05_linechart_en.md) | Next: [HeatmapChart](07_heatmapchart_en.md) | [Русский](06_columnchart_ru.md)

---

# ColumnChart (Column / Bar)

Shows vertical columns or horizontal bars. Multiple series are grouped side by side within each category slot.

`RimUI.Components.ColumnChart : Chart` (sealed)

## Example

```csharp
var column = new ColumnChart { Style = { Height = 240f } };
column.Series.Add(new ChartSeries("Shift A", 12f, 19f, 8f, 15f));
column.Series.Add(new ChartSeries("Shift B", 9f, 14f, 11f, 10f));
var bar = new ColumnChart { Horizontal = true };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Horizontal` | `bool` | `false` | Uses horizontal bars instead of vertical columns. |
| `GroupGapFraction` | `float` | `0.28` | Category-slot fraction between groups. |
| `BarGapFraction` | `float` | `0.10` | Group-width fraction between bars. |
| `ShowGrid` | `bool` | `true` | Shows grid. |
| `GridLines` | `int` | `4` | Grid line count. |

Bars use regular `Background` commands and work without GL polygon primitives. No public methods or events beyond `Chart`.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style / sprite / animation | See `Chart` | Shared behavior. |
| Multiple series | Yes | Grouped within category slots. |
| Tooltip | Yes | Each bar has a hotspot. |


---

[Table of contents](../index_en.md) - Charts | Previous: [LineChart (Line / Area)](05_linechart_en.md) | Next: [HeatmapChart](07_heatmapchart_en.md) | [Русский](06_columnchart_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
