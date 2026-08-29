![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.9` · mod `0.2.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [Icon](07_icon_en.md) | Next: [Text](09_text_en.md) | [Русский](08_icons_ru.md)

---

# Icons

The framework's built-in icon sheet and factory for `Icon` elements. The default atlas uses 48 x 48 cells, 8 columns, and 1 px gaps. This is a static helper, not a UI element.

`RimUI.Elements.Icons` (static class)

## Example

```csharp
var ic = Icons.Get(Icons.Check, 16f);
ic.Tint = ColorRGBA.White;

var rotated = Icons.Get(Icons.ChevronRight, 20f);
rotated.Rotation = 45f;
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `DefaultAtlas` | `string` | `"RimUI/Icons"` | Built-in atlas key. |
| `DefaultCell` | `int` | `48` | Cell size in pixels. |
| `DefaultColumns` | `int` | `8` | Atlas columns. |
| `Gap` | `int` | `1` | Gap between cells. |
| `AtlasKey` | `string` | `DefaultAtlas` | Active atlas. |
| `Cell` | `int` | `DefaultCell` | Active cell size. |
| `Columns` | `int` | `DefaultColumns` | Active column count. |

Named indices: `Close=0`, `Hamburger=1`, `Check=2`, `ChevronDown=3`, `ChevronRight=4`, `ChevronUp=5`, `ChevronLeft=6`, `Dot=7`, `Plus=8`, `Minus=9`, `SpinnerRing=10`, `Info=11`, `Warning=12`, `Error=13`, `ChevronDoubleUp=14`, `ChevronDoubleDown=15`, `ChevronDoubleRight=16`, `ChevronDoubleLeft=17`, `Gear=18`.

### Exact `Rect(index)` formula

```text
col = index % Columns
row = index / Columns
x = col * (Cell + Gap)
y = row * (Cell + Gap)
Rect = (x, y, Cell, Cell)
```

Indices run left-to-right, top-to-bottom from zero. The default stride is 49 px while the returned rectangle remains exactly 48 x 48. Coordinates are top-left pixel coordinates; the rendering adapter later converts them to Unity UVs and flips Y.

## Styles

Not applicable to a static helper.

## Methods

| Method | Returns | Description |
|---|---|---|
| `static Get(int index, float size = 0f)` | `Icon` | Creates an icon with `Source = Rect(index)` and exact size when `size > 0`. |
| `static Rect(int index)` | `RectF` | Returns the icon's pixel rectangle. |
| `static Configure(string atlasKey, int cell, int columns)` | `void` | Replaces the whole icon sheet, used by `ThemeManager`. |
| `static ResetConfig()` | `void` | Restores defaults. |

## Events

Not applicable.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Custom icon theme | Yes | `Configure` replaces the atlas. |
| Sprite, animation, disabled, hover fade | See `Icon` | These belong to returned elements. |


---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [Icon](07_icon_en.md) | Next: [Text](09_text_en.md) | [Русский](08_icons_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
