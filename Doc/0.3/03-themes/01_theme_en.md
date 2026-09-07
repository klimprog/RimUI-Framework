![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.3.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Themes | Previous: [SpriteFrame — sprite frames and tiling](../02-styles/02_spriteframe_en.md) | Next: [Animations — module registry](../04-animations/01_animations_en.md) | [Русский](01_theme_ru.md)

---

# Theme — themes and theme files

A theme provides default styles for every component through named **slots**, plus a color palette
and layout metrics. Components do not need to set every value in their own `Style`; unset values
are pulled from the active theme slot every frame. The built-in dark theme uses a warm,
vanilla-RimWorld palette and works even without a theme file. Theme files are JSON parsed by
RimUI's own parser rather than `UnityEngine.JsonUtility`, which cannot handle the nested
structures used here. Parsing is strict: all errors are collected, and a theme with any errors is
rejected as a whole.

`RimUI.Styling.Theme` (data) + `RimUI.Adapter.ThemeManager`/`ThemeJson` (loading)

## Example

```csharp
string err;
if (!RimUI.Adapter.ThemeManager.LoadTheme("MyTheme", myModRootDir, out err))
    Log.Message("Could not load theme: " + err);
```

```json
{
  "name": "MyTheme",
  "palette": {
    "surface": "#1A1713", "surfaceAlt": "#2E2A23", "accent": "#BE9E63",
    "borderColor": "#675F50", "textColor": "#E8E3D8", "textMuted": "#A39C8B"
  },
  "metrics": {
    "radiusSmall": 4, "radiusMiddle": 8, "radiusLarge": 16,
    "borderSmall": 1, "borderMiddle": 2, "borderLarge": 3,
    "maxWindowHeightFraction": 0.85, "hoverFadeDuration": 0.12
  },
  "checkbox": {
    "box": { "background": "#232018", "radius": "small", "borderWidth": "small" },
    "box_on": { "background": "linear:vertical:#BE9E63:#8E7A50" }
  }
}
```

To export a complete theme template with every slot and current value, open the RimUI Framework
mod settings and click **Copy theme template**.

## Parameters (Theme palette and metrics)

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Name` | `string` | `"default"` | Theme name. |
| `Surface` | `ColorRGBA` | warm dark | Window background. |
| `SurfaceAlt` | `ColorRGBA` | lighter warm dark | Section/panel background. |
| `Accent` | `ColorRGBA` | warm gold | Accent color for active controls. |
| `BorderColor` | `ColorRGBA` | warm gray-brown | Default border color. |
| `TextColor` | `ColorRGBA` | warm white | Primary text color. |
| `TextMuted` | `ColorRGBA` | muted | Secondary text color. |
| `SpacingUnit` | `float` | `8` | Base spacing unit. |
| `RadiusSmall`/`Middle`/`Large` | `float` | `4`/`8`/`16` | Pixel values behind `BorderRadius` variants. |
| `BorderSmall`/`Middle`/`Large` | `float` | `1`/`2`/`3` | Pixel values behind `BorderWidth` variants. |
| `MaxWindowHeightFraction` | `float` | `0.85` | Maximum window height as a fraction of screen height. |
| `HoverFadeDuration` | `float` | `0.12` | Hover transition duration in seconds; `0` is immediate. |

## Theme JSON structure

| Top-level key | Contents |
|---|---|
| `palette` | `surface`, `surfaceAlt`, `accent`, `borderColor`, `textColor`, `textMuted`. |
| `metrics` | `radiusSmall/Middle/Large`, `borderSmall/Middle/Large`, `maxWindowHeightFraction`, `hoverFadeDuration`. |
| `text`, `field`, `button`, `divider`, `popover`, `menu`, `tooltip`, `modal` | Named top-level slots; `modal` also supports `sectionGap`. |
| `buttonPresets` | `defaultBg/dangerBg/warningBg/successBg/text/textDisabled/hoverShift/pressShift`. |
| `menuItems` | `textColor/textSize/hover/disabledText/height/iconSlot/minWidth`. |
| `input` | `bg/border/borderFocus/placeholder`. |
| `control` | `bg/border/accent/handle/disabled` for checkboxes, radio buttons, switches, and sliders. |
| Sections prefixed with `#` (`"#card"`, `"#danger"`, ...) | **Style classes** - reusable sets of properties that attach to any element. See "Style classes" below. |
| Other sections (`"checkbox"`, `"slider"`, `"table"`, etc.) | Component slots registered in `ThemeSlots`; each component page lists its paths. The section root is the `"component"` slot, while nested objects are `"component/part"` slots. |

### Field value formats

| Value type | Format |
|---|---|
| Color | `"#RRGGBB"` or `"#RRGGBBAA"`. |
| Fill | A color; `"linear:vertical\|horizontal\|diagonal:#color1:#color2"`; or `"image:Key[:fit[:ax,ay[:#backColor[:#tint]]]]"`. |
| Size (radius/borderWidth) | `"small"` \| `"middle"` \| `"large"`. |
| Spacing | `"all"` \| `"horizontal vertical"` \| `"left top right bottom"`. |
| Shadow | `"offsetX offsetY blur spread [#color]"`. |
| Sprite | `"AtlasKey [N [N \| left top right bottom]]"` or `{ atlas, corner, cornerLeft/Top/Right/Bottom }`; `N` is the corner/edge size in units (1 unit = 4 texture px). See SpriteFrame for the atlas format: 3×3 tiles, 1 px gaps, and the sheet-size formula. |
| Text | `textSize` (`tiny\|small\|medium`), `textAlign` (`left\|center\|right`), `textValign` (`top\|middle\|bottom`), `textWrap` (`true\|false`), `textWeight` (`normal\|bold`). |

### Special values

| Value | Meaning |
|---|---|
| `"none"` | Unset. Continue down the priority chain: code → theme file → component defaults. |
| `"off"` | Explicitly disabled. Stop fallback: do not draw the background/shadow/sprite, or resolve spacing to 0. |
| Key prefixed with `_` | Ignored, useful for JSON comments such as `"_comment"`. |

## Style classes

A class is a named set of style properties that attaches to any element without touching either
its type or its theme slot. It solves what previously had to be done by rewriting the style in
code: a slot is chosen by the element's **type** and cannot be substituted from outside, so there
was no way to say "make this panel a card and that one a warning".

Classes are declared in the theme as sections prefixed with `#`. The prefix is what tells them
apart from slots: slots are named by paths such as `table/row`, and without a marker a class would
be indistinguishable from a typo in a slot name.

```json
{
  "#card": {
    "bg": "#2A2E35",
    "radius": "middle",
    "padding": "12",
    "borderWidth": "small",
    "borderColor": "#3C4149"
  },
  "#selected": { "borderColor": "#C8A45C" }
}
```

A class accepts the same fields as a component slot.

A class can also be declared from code, with no theme file:

```csharp
ctx.Theme.DefineClass("card", new Style { Radius = BorderRadius.Middle, Padding = new Thickness(12f) });
```

### Attaching to an element

```csharp
var p = new Panel();
p.AddClass("card");
p.SetClass("selected", isSelected);   // switch on or off in one call
```

| Element method | Description |
|---|---|
| `Classes` | The list of classes. Order matters: each one overrides the previous. |
| `AddClass(name)` | Adds it unless already present. |
| `RemoveClass(name)` | Removes it. |
| `SetClass(name, on)` | Switches it on or off - handy for states that change from frame to frame. |
| `HasClass(name)` | Checks for it. |

The list can be changed on the fly: the style is recomputed every frame, so a class change shows
from the very next one. A class the theme does not declare is simply ignored — that is not an
error.

### Order of application

Each level overrides the previous one:

```
defaults from code
        v
theme slot
        v
classes (in list order: first, then the second on top of it, ...)
        v
the element's explicit Style
```

So a property set explicitly in code beats any class, and a class beats the theme slot.

## Methods

| Method | Returns | Description |
|---|---|---|
| `ThemeManager.LoadTheme(string name, string modRootDir, out string error)` | `bool` | Load and apply a theme; returns `false` on a parse error, with details in `error`. |
| `Theme.Slot(string path)` | `Style` | Get a slot such as `"checkbox/box"`; `null` if undeclared. |
| `Theme.DefineClass(string name, Style style)` | - | Declare a style class from code. |
| `Theme.Class(string name)` | `Style` | Get a class by name; `null` if undeclared. |
| `Theme.ClearClasses()` | - | Remove every class. |
| `Theme.RadiusPx(BorderRadius r)` | `float` | Convert a radius variant to theme pixels. |
| `Theme.BorderPx(BorderWidth w)` | `float` | Convert a border-width variant to theme pixels. |
| `static Theme.BuildDefault()` | `Theme` | Build the default theme embedded in code. |

## Events

Not applicable. Theme loading is synchronous: `LoadTheme` returns the result directly and does not
use callbacks. All open windows pick up a changed active theme without restarting because style
resolution runs again every frame.

### Exact slot rebuild order and live theme switching

`RebuildSlots()` runs exactly **once** while loading a theme file, after parsing its palette and
metrics but before applying named JSON slot sections. It resets the slot registry, rebuilds every
registered slot from the current palette, rebuilds named top-level slots (`TextSlot`, `Field`,
`Button`, and popup/menu/tooltip panels), then resets window panels (`WindowPanel`, `ModalPanel`,
and others) to the native backing until the theme file explicitly replaces them.

Live switching through `ThemeManager.Apply` only replaces the active Theme reference and
invalidates the baked-texture cache. Slots are not rebuilt again because that already happened
while parsing the new file. Every window reads the active theme again before `Measure` on each
frame, so the new theme normally appears in the same frame. A one-frame delay is theoretically
possible if a click changes the theme **after** that window has already completed `Measure` for the
frame. Its `Emit` then uses the old style once, and the full change appears on the next frame. This
is not an explicit framework guarantee; it follows from the normal `Measure → Arrange → Emit`
order.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Component slots | Yes | The main theming mechanism; see individual component pages. |
| Sprites | Yes | Use a string or object in JSON. |
| String animation | Yes | Use `"animation"` in any JSON slot. |
| Live reload | Yes | Switch themes without reopening windows. |


---

[Table of contents](../index_en.md) - Themes | Previous: [SpriteFrame — sprite frames and tiling](../02-styles/02_spriteframe_en.md) | Next: [Animations — module registry](../04-animations/01_animations_en.md) | [Русский](01_theme_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
