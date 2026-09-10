![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.31` · mod `0.3.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Charts | Previous: [RadarChart](02_radarchart_en.md) | Next: [PieChart (Pie / Donut)](04_piechart_en.md) | [Русский](03_polarchart_ru.md)

---

# PolarChart (Polar Area)

Categories split the circle into equal sectors; sector radius uses values from **`Series[0]` only**. Its legend shows categories rather than series.

`RimUI.Components.PolarChart : Chart` (sealed)

## Example

```json
{ "labels": ["Sleep", "Recreation", "Work", "Health", "Mood"],
  "series": [ { "name": "Colonist", "values": [70, 35, 85, 60, 50] } ] }
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `GridRings` | `int` | `4` | Grid rings. |
| `FillAlpha` | `float` | `0.55` | Sector fill alpha. |
| `ShowScale` | `bool` | `true` | Shows scale labels on the top axis. |

Uses only `Series[0]`. No public methods or events beyond `Chart`.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style / sprite / animation | See `Chart` | Shared behavior. |
| Multiple series | No | Only `Series[0]`. |
| Tooltip | Yes | Sector hotspots. |
| Legend | Special | Shows `Labels` rather than series. |


---

[Table of contents](../index_en.md) - Charts | Previous: [RadarChart](02_radarchart_en.md) | Next: [PieChart (Pie / Donut)](04_piechart_en.md) | [Русский](03_polarchart_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
