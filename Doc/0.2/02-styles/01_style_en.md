![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Styles and sprites | Previous: [FlexBox](../01-grid/03_flexbox_en.md) | Next: [SpriteFrame — sprite frames and tiling](02_spriteframe_en.md) | [Русский](01_style_ru.md)

---

# Style — style model

Every `UiElement` exposes a public `Style Style` field. Style is data; it draws nothing by itself.
The explicit style and theme slot are resolved every frame, so switching themes updates windows
that are already open. For most fields, `null` means "not specified; use the theme slot."
Explicit values such as `Fill.Off`, `BoxShadow.Off`, and `BorderWidth.None` deliberately disable
that feature and stop the fallback chain.

`RimUI.Styling.Style` (sealed class)

## Example

```csharp
el.Style.Margin = new Thickness(8f);
el.Style.Padding = new Thickness(12f, 8f);
el.Style.Width = 200f;                 // 0 = auto (content/grid share)
el.Style.Background = Fill.Solid(new ColorRGBA(0.2f, 0.3f, 0.5f, 1f));
el.Style.Radius = BorderRadius.Middle;
el.Style.BorderWidth = BorderWidth.Small;
el.Style.BorderColor = ColorRGBA.White.WithAlpha(0.4f);
el.Style.Shadow = new BoxShadow { Offset = new Vec2(0, 2), Blur = 8f, Color = new ColorRGBA(0, 0, 0, 0.45f) };
el.Style.Text = new TextStyle { Align = TextAlign.Center, Color = ColorRGBA.White };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Margin` | `Thickness` | `Thickness.Zero` | Outer spacing. |
| `Padding` | `Thickness` | `Thickness.Zero` | Inner spacing. |
| `Width` | `float` | `0` | Explicit width; 0 means auto. Snapped to the nearest multiple of 3; see below. |
| `Height` | `float` | `0` | Explicit height; 0 means auto. Uses the same multiple-of-3 rule. |
| `MinHeight` | `float` | `0` | Minimum height when `Height` is not set. |
| `Mode` | `StyleMode` (`Styled`\|`Sprite`) | `Styled` | Rendering path; in practice, setting `Sprite` switches this automatically. |
| `Background` | `Fill` | `null` | Background fill; `null` uses the theme. |
| `Radius` | `BorderRadius?` (`None`\|`Small`\|`Middle`\|`Large`) | `null` | Corner radius; pixel values come from the theme. |
| `BorderWidth` | `BorderWidth?` (`None`\|`Small`\|`Middle`\|`Large`) | `null` | Border width. |
| `BorderColor` | `ColorRGBA?` | `null` | Solid border color. |
| `BorderFill` | `Fill` | `null` | Border fill, allowing a separate border gradient. |
| `Shadow` | `BoxShadow` | `null` | Shadow behind the element. |
| `Sprite` | `SpriteFrame` | `null` | 9-slice frame; see SpriteFrame. When set, this Style's background, border, radius, and gradient are ignored. |
| `AlignItems` | `AlignItems` (`Start`\|`Center`\|`End`\|`Stretch`) | `Stretch` | Cross-axis child alignment for containers. |
| `AlignSelf` | `AlignItems?` | `null` | Per-child alignment override, like CSS `align-self`. |
| `JustifyContent` | `JustifyContent` (`Start`\|`Center`\|`End`\|`SpaceBetween`\|`SpaceAround`\|`SpaceEvenly`) | `Start` | Main-axis child distribution. |
| `Gap` | `float` | `0` | Space between container children. |
| `Left`/`Top`/`Right`/`Bottom` | `float?` | `null` | Absolute offsets relative to the parent, like CSS. |
| `Anchor` | `Anchor` (`TopLeft`\|`TopCenter`\|`TopRight`\|`MiddleLeft`\|`MiddleCenter`\|`MiddleRight`\|`BottomLeft`\|`BottomCenter`\|`BottomRight`) | `TopLeft` | Anchor used when offsets are absent; all 9 positions of a 3×3 grid. |
| `OverflowX`/`OverflowY` | `Overflow` (`Visible`\|`Clip`\|`Scroll`) | `Visible` | Overflow behavior. |
| `Text` | `TextStyle` | `null` | Text size, color, alignment, and shadow; see below. |
| `Animation` | `string` | `null` | Animation spec such as `"pulse 1.2 loop"`; see Animations. |
| `Cursor` | `CursorKind?` | `null` | Cursor over the element; `null` = unset (inherited from the parent). See the Cursor section below. |

### TextStyle (`Style.Text`)

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Size` | `FontSize` (`Tiny`\|`Small`\|`Medium`) | `Small` | Native RimWorld font size. |
| `Weight` | `FontWeight` (`Normal`\|`Bold`) | `Normal` | Font weight. |
| `Color` | `ColorRGBA` | `White` | Text color. |
| `Align` | `TextAlign` (`Left`\|`Center`\|`Right`) | `Left` | Horizontal alignment. |
| `VAlign` | `VerticalAlign` (`Top`\|`Middle`\|`Bottom`) | `Top` | Vertical alignment. |
| `Wrap` | `bool` | `true` | Enable line wrapping. |
| `FontFamily` | `string` | `null` | OS font name; `null` uses the game's native font. |
| `PixelSize` | `float` | `0` | Exact pixel size overriding `Size`; 0 uses the native size. Reliable with `Tiny` and `Small`, not `Medium`, due to a RimWorld limitation. |
| `ShadowOffset` | `Vec2` | `(1,1)` | Text shadow offset. |
| `ShadowColor` | `ColorRGBA` | transparent | Shadow color; alpha 0 disables it. |

### Fill (`Style.Background`/`BorderFill`)

| Parameter | Type | Description |
|---|---|---|
| `Kind` | `FillKind` (`None`\|`Solid`\|`Gradient`\|`Image`) | Fill type. |
| `Color` | `ColorRGBA` | Color used by `Solid`. |
| `Direction` | `GradientDirection` (`Vertical`\|`Horizontal`\|`Diagonal`) | Gradient direction. |
| `Stops` | `GradientStop[]` | Gradient stops: offset 0..1 and color. |
| `SpriteKey` | `string` | Texture key used by `Image`. |
| `Fit` | `ImageFit` (`Stretch`\|`Cover`\|`Contain`\|`Auto`) | Image scaling mode. |
| `ImageAnchor` | `Vec2` | Image anchor from 0..1; (0.5, 0.5) is centered. |
| `ImageTint` | `ColorRGBA` | Image tint. |
| `BackColor` | `ColorRGBA` | Color behind transparent image pixels. |

## Cursor

`Style.Cursor` sets the **meaning** of the cursor, while the actual texture and hotspot come from
the theme (`Theme.Cursors`). The kinds are:

| Value | Meaning |
|---|---|
| `None` | Do not request a cursor: the element is transparent to this mechanism (the default is expressed as `null`). |
| `Arrow` | An explicit arrow. Not the same as `None`: `Arrow` **overrides** the parent's cursor. |
| `Hand` | Clickable: buttons, window headers, menu entries. |
| `Move` | Dragging (the system SIZEALL cursor). |
| `Text` | Text entry. |
| `ResizeH`, `ResizeV`, `ResizeNWSE`, `ResizeNESW` | Resizing horizontally, vertically and along both diagonals. |

How it works: every element under the mouse requests its cursor, **the deepest one wins**, and the
kind is applied once per frame. Disabled elements (`EffectiveDisabled`) request nothing — a hand
over an unavailable button would be a lie.

The theme supplies these by default: a hand for buttons (`button`) and select fields
(`select/field`), a caret for input fields (`input/frame`), and a hand for the header of a
draggable window. It can be turned off through the theme:

```json
{ "button": { "cursor": "none" } }
```

Values for a JSON theme: `none`, `arrow`, `hand`, `move`, `text`, `resize-h`, `resize-v`,
`resize-nwse`, `resize-nesw`.

> OS cursors are not available to mods: at runtime Unity only offers `SetCursor(texture, hotspot)`,
> and the named system cursors live in the editor. So the framework ships its own assets drawn in
> the system style — a white body with a black outline. Replace them by swapping the entries in
> `Theme.Cursors`.

## Multiples of 3 (`Width`/`Height`)

Explicit `Width` and `Height` values snap to the nearest multiple of 3 framework units. Sprite
tiling uses a 3-unit decorative module equal to 12 texture pixels, so edges and backgrounds can
repeat in whole tiles without clipping. `Margin` and `Padding` are not snapped. The formula is
`Math.Round(v/3, MidpointRounding.AwayFromZero) * 3`:

| Input | Result |
|---|---|
| 1 | 0 |
| 2 | 3 |
| 28 | 27 |
| 29 | 30 |
| 30 | 30 |
| 31 | 30 |

**Watch out:** `Width = 0` means "unset/auto" to `StyleMerge`. Any value in `(0, 1.5)` snaps to
0 and silently becomes auto width instead of a tiny explicit size. For example,
`Style.Width = 1f` is effectively ignored. The switch happens at 1.5: `v/3 = 0.5` rounds away
from zero to 1, producing 3; anything just below 1.5 produces 0.

## Exact resolution order (StyleMerge)

Each frame, the final style combines two layers: the element's own **code style** (`Style`) over
the **theme slot**. The slot already includes the theme-file-over-built-in-defaults priority,
which is resolved when the theme is loaded. What counts as "unset" depends on the field type:

- Reference and nullable fields (`Background`, `Radius`, `BorderWidth`, `BorderColor`,
  `BorderFill`) use normal `own ?? slot` fallback.
- `Text` (`TextStyle`) merges as a **whole object**, not field by field. If the element provides
  `Style.Text`, the entire slot TextStyle, including size, alignment, font, and shadow, is dropped.
- `Width`/`Height` use `0` as "unset"; see the snapping trap above.
- For `Margin`/`Padding`, any negative component is explicit `Off` and resolves to zero; any
  nonzero component makes the whole value explicit; all zeros fall back to the slot.
- `AlignItems`/`JustifyContent` are considered unset when equal to their enum defaults,
  `Stretch`/`Start`. As a result, **you cannot explicitly set `AlignItems.Stretch` or
  `JustifyContent.Start` to override a non-default theme slot**. Those values are indistinguishable
  from unset and still fall through to the slot.
- Negative `Gap` is explicit `Off` and resolves to 0; positive values use `own`; zero falls back
  to the slot.
- `Animation` uses `own ?? slot`, but an empty string `""` is not `null`, so it explicitly
  overrides and disables the slot animation. In theme JSON, this corresponds to `"off"`.
- Positioning (`Left/Top/Right/Bottom/Anchor`) and overflow (`OverflowX/OverflowY`) come **only**
  from element code. Themes do not affect or support them.
- `AlignSelf` does not participate in general resolution. FlexBox reads it directly from the
  child's raw `Style`; a theme slot cannot set it. See FlexBox.

## Styles

This is the style model itself. Component-specific theme slots are listed on component pages.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Clone()` | `Style` | Deep copy, including nested `Text`. |
| `static SnapUnit(float v)` | `float` | Snap a size to the nearest multiple of 3; zero and negatives are unchanged. |
| `SetExactSize(float w, float h)` | `void` | Internal exact sizing without snapping, used for computed layout areas where snapping would create gaps or overlap. |
| `HasVisualBackground` (property) | `bool` | Whether the style has a visible background. |
| `HasVisualBorder` (property) | `bool` | Whether the style has a visible border. |

## Events

Not applicable. Style is data, not an interactive element.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Theme slots | Yes | Anything not explicitly set falls back to the component's theme slot. |
| Sprite frame | Yes | Use `Sprite`; see SpriteFrame. |
| String animation | Yes | Use `Animation`. |
| Explicit `off` | Yes | `Fill.Off`, `BoxShadow.Off`, and `BorderWidth.None` stop the fallback chain. |


---

[Table of contents](../index_en.md) - Styles and sprites | Previous: [FlexBox](../01-grid/03_flexbox_en.md) | Next: [SpriteFrame — sprite frames and tiling](02_spriteframe_en.md) | [Русский](01_style_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
