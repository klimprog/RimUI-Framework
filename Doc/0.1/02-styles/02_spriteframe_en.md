![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Styles and sprites | Previous: [Style — style model](01_style_en.md) | Next: [Theme — themes and theme files](../03-themes/01_theme_en.md) | [Русский](02_spriteframe_ru.md)

---

# SpriteFrame — sprite frames and tiling

A sprite frame uses a 9-slice atlas instead of tinted white templates. Assign it through
`Style.Sprite`. When a Sprite is set, the same Style's background, border, radius, and gradient are
ignored; the sprite replaces them rather than layering with them.

`RimUI.Styling.SpriteFrame` (sealed class)

## Atlas texture format

An atlas is **one image** laid out as a **3×3 tile grid**: 4 corners, 4 edges, and a center, for 9
sub-sprites total. Neighboring tiles must be separated by **exactly 1 fully transparent pixel**
(alpha 0). Without that gap, Unity's bilinear filtering pulls color from the neighboring tile and
creates a blurry seam.

**Image requirements:**

| Requirement | Value | Required? |
|---|---|---|
| Sheet shape | Square: width = height | Strict in practice, though not validated. Tile size `T` is calculated from width only and applied to both axes; a different height shifts or clips vertical tiles. |
| Sheet side | `3·T + 2`, where `T` is one tile side in pixels | Fixed formula. |
| Tile size `T` | Multiple of 4 | Recommended, not validated; otherwise the edge size in framework units maps to fractional pixels. |
| Gap between tiles | Exactly 1 px, alpha 0 | Required to prevent blurry seams. |
| All 9 tiles | Same `T×T` size | Required; rows and columns share one formula. |

**Units.** Frame sizes (`Edge`, `Reserve`) use framework units, not texture pixels:
**1 unit = 4 texture pixels**. A `T = 48 px` tile contains `48 / 4 = 12` units, the maximum that
can be sampled from one side for an edge or corner.

The bundled atlas at `RimUI Framework/RimUIThemes/Default/assets/demo/pattern.png` is
**146×146 px**. The formula gives `T = (146 − 2) / 3 = 48 px`, an integer divisible by 4. The
columns and rows at x/y = 48 and 97 are fully transparent one-pixel separators.

## How the frame is sampled

```csharp
el.Style.Sprite = new SpriteFrame("demo/pattern", 3f);
```

`SpriteFrame(atlasKey, corner: 3f)` produces `Edge = 1` and `Reserve = 2`. The total side edge is
`N = Edge + Reserve = 3` units, or `3 × 4 = 12` pixels.

1. `N` is capped at `T / 4` units because a corner cannot be larger than one complete atlas tile.
   Larger values are silently clamped with no error or warning.
2. A strip `N × 4` pixels wide is sampled **from the tile's inner edge**, the side nearest the
   center of the sheet. If `N < T`, decoration farther toward the outer edge is not included.
3. Corner pieces are always drawn at exactly the requested `N`-unit size and never stretch to fit
   the element. Edges and center are flexible: by default they stretch across the remaining area,
   or they repeat as whole tiles when `Repeat = true`.

### Edge versus Reserve

Together, `Edge` and `Reserve` decide how many pixels are sampled (`N = Edge + Reserve`). They are
indistinguishable in the texture itself and form one continuous strip. Their difference only
affects layout:

- **`Edge`** is the functional frame edge **on** the element boundary. It insets content and is
  added to `Padding`.
- **`Reserve`** extends **outside** the element boundary. It does not inset content; instead, it is
  reserved as minimum margin so decorative overhang does not overlap neighboring grid elements.

The mod showcase page Styles → Sprite frame visualizes this: blue marks the functional `Edge`
around the content, while gold marks the decorative `Reserve` extending outward.

## Example

```csharp
el.Style.Sprite = new SpriteFrame("MyMod/PanelFrame", corner: 12f);

// Separate functional edge and decorative reserve.
el.Style.Sprite = new SpriteFrame("MyMod/OrnateFrame",
    edge: new Thickness(1f), reserve: new Thickness(3f));

// Tile edges and center instead of stretching them.
el.Style.Sprite = new SpriteFrame("demo/pattern", 3f) { Repeat = true };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `AtlasKey` | `string` | — | Path under `Textures/` or a key registered by the theme. |
| `Edge` | `Thickness` | `1` on every side | Functional edge in units (1 unit = 4 texture px); sits on the element boundary and insets content. |
| `Reserve` | `Thickness` | `Thickness.Zero` | Decorative reserve in units; extends outside the element and becomes margin rather than content inset. |
| `Repeat` | `bool` | `false` | Tile edges and center from the start of each side. The final partial tile is clipped, not squashed. `false` stretches them across the area. |
| `Corner` (property) | `Thickness` | computed | Total side corner/edge size, `Edge + Reserve`, in units. |

## Styles

SpriteFrame is not a standalone visual slot. Assign it through any element's `Style.Sprite`; the
presence of a Sprite switches that element to `StyleMode.Sprite`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `SpriteFrame()` (constructor) | — | Empty frame; assign fields manually. |
| `SpriteFrame(string atlasKey)` (constructor) | — | Atlas key only, with no edge or reserve. |
| `SpriteFrame(string atlasKey, float corner)` (constructor) | — | Uniform side size in units: `Edge = 1`, `Reserve = corner - 1`. |
| `SpriteFrame(string atlasKey, Thickness edge, Thickness reserve)` (constructor) | — | Separate values per side. |
| `static SpriteFrame.Off` (field) | `SpriteFrame` | Explicitly disabled, even if the theme slot provides a frame. |

## Events

Not applicable. SpriteFrame is data, not an interactive element.

## Tiling (`Repeat = true`) — exact behavior

Without Repeat, edges and center stretch across the available area. With `Repeat = true`, they are
drawn as whole tiles instead:

1. The source tile, either the full `T×T` center or an edge strip, is converted to framework units
   using 1 unit = 4 px.
2. Repeat count on each axis is the **ceiling** of `screen area size / tile size` in framework
   units. This covers the area with whole tiles and usually one final partial tile.
3. **The final tile is clipped to the remaining space, not squashed.** If only 70% fits, exactly
   70% of the source width or height is sampled from the start. The pattern keeps its proportions;
   the last repeat is simply incomplete.
4. Corners never tile. They are always drawn once as indivisible, fixed-size pieces regardless of
   `Repeat`.
5. Every tile boundary snaps to a whole screen pixel to prevent seams at fractional UI scales such
   as 1.25 or 1.5.

Practical tip: use a larger source tile `T` for large areas. Larger tiles mean fewer draw repeats
and usually make the seam at the final partial tile less noticeable.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Theme slots | Yes | Set through code (`Style.Sprite = ...`) or theme JSON as a string or object. |
| Tiling | Yes | Use `Repeat`; see above. |
| Animation | Indirectly | The owning element's `Style.Animation` works independently of the sprite. |
| Disabled/hover fade | Not applicable | This is static frame data, not an interactive element. |


---

[Table of contents](../index_en.md) - Styles and sprites | Previous: [Style — style model](01_style_en.md) | Next: [Theme — themes and theme files](../03-themes/01_theme_en.md) | [Русский](02_spriteframe_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
