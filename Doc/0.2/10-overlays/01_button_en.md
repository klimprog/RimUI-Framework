![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.9` · mod `0.2.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [OrganizationChart](../09-data/10_organizationchart_en.md) | Next: [Popover](02_popover_en.md) | [Русский](01_button_ru.md)

---

# Button

A content-sized button with color presets, optional content on both sides, disabled state, and a native RimWorld tooltip. It does not stretch horizontally.

`RimUI.Elements.Button : UiElement` (sealed)

## Example

```csharp
var b = Button.Make("Save");
b.OnClick = () => Save();
b.Preset = ButtonPreset.Success;

var iconBtn = new Button { Left = Icons.Get(Icons.Close, 16f), Content = new Text("Close") };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Left` | `UiElement` | `null` | Left content, usually an icon. |
| `Content` | `UiElement` | `null` | Main content. |
| `Right` | `UiElement` | `null` | Right content. |
| `OnClick` | `Action` | `null` | Click callback. |
| `Disabled` | `bool` | `false` | Disables the button. |
| `DisabledWhen` | `Func<bool>` | `null` | Dynamic disabled condition checked every frame. |
| `Preset` | `ButtonPreset` | `Default` | Color preset. |
| `Tooltip` | `string` | `null` | Native RimWorld tooltip, not framework `TooltipBox`. |

`ButtonPreset` values: `Default` uses `Theme.ButtonDefaultBg`, `Danger` uses red, `Warning` yellow-orange, and `Success` green.

## Exact hover fade and color shifts

Hover is a linear time interpolation at `speed = 1 / Theme.HoverFadeDuration`. With the default `0.12`, 0 to 1 takes exactly 0.12 seconds independently of FPS. State persists between frames, so reversing hover midway continues from the current value.

The result is `Lerp(baseColor, shiftedColor, k)`. Shift adds directly to each RGB channel and clamps to `[0,1]`; alpha is unchanged. `ButtonHoverShift = 0.08` and `ButtonPressShift = -0.07`. Pressed and disabled states are immediate. Disabled color instead lerps 55% toward the RGB average to desaturate it.

## Styles

The state sprite comes from `button.sprite` for `ButtonState.Default`, `Hover`, `Pressed`, or `Disabled`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Button()` (constructor) | - | Creates an empty button. |
| `static Make(string text, string leftIcon = null, string rightIcon = null)` | `Button` | Creates centered, vertically aligned, non-wrapping text and optional icons. |
| `SetStyle(string path, string value)` | - | Applies the path to the root style. Invalid paths are ignored. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnClick` | `Action` | - | A click while not disabled. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the element bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective state from `Disabled` or `DisabledWhen` changes. |

Universal click events run independently of `OnClick`.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Defaults from `Theme.Button`. |
| Sprite frame | Yes | `button.sprite` by state. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled | Yes | Static or dynamic; gray background and disabled text color. |
| Hover fade | Yes | Hover fades; pressed and disabled are immediate. |


---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [OrganizationChart](../09-data/10_organizationchart_en.md) | Next: [Popover](02_popover_en.md) | [Русский](01_button_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
