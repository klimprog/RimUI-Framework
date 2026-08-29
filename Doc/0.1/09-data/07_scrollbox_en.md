![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Data | Previous: [TreeTable](06_treetable_en.md) | Next: [Button](../10-overlays/01_button_en.md) | [Русский](07_scrollbox_ru.md)

---

# ScrollBox

Wraps one child in vertical scrolling when its content may exceed the available height.

`RimUI.Elements.ScrollBox : UiElement`

## Example

```csharp
var scroll = new ScrollBox(myTallContent) { Key = "myScroll" };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Child` | `UiElement` | `null` | Single contained element. |
| `ScrollbarWidth` | `float` | `10` | Scrollbar width. |
| `WheelStep` | `float` | `40` | Multiplier applied to wheel delta. |
| `TrackColor` | `ColorRGBA` | `(1,1,1,0.06)` | Scrollbar track color. |
| `HandleColor` | `ColorRGBA` | `(1,1,1,0.25)` | Handle color. |
| `HandleActiveColor` | `ColorRGBA` | `(1,1,1,0.45)` | Hovered or dragged handle color. |

## Styles

Supports the standard container fields: `Background`, border, shadow, sprite, padding, and margin.

## Methods

| Method | Returns | Description |
|---|---|---|
| `ScrollBox()` (constructor) | - | Creates an empty box; assign `Child` manually. |
| `ScrollBox(UiElement child)` (constructor) | - | Creates a box with content. |
| `SetStyle(string path, string value)` | `void` | Applies a path such as `background` or `radius` to the root style. |

Wheel input is handled after children, so a nested scroller gets priority. Drag the handle or click the track to jump.

The scrollbar appears when `Child.Desired.Height > availableHeight + 0.5`. The 0.5 px epsilon prevents float-boundary flicker. `WheelStep` multiplies the engine's raw `Event.delta.y` and may produce fractional movement on trackpads. It applies only to `ScrollBox`; lists, tables, trees, and selects use their own hardcoded 24-28 multipliers.

## Events

No custom or selection events are exposed.

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |

`Events.Scroll` is raised separately from the component's own content scrolling. `ScrollBox` has no `Disabled` field.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Standard background, border, shadow, sprite, and spacing. |
| Sprite frame | Yes | Explicit `Style.Sprite` rendering path. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled | No | No disabled state. |
| Hover fade | No | Handle color switches immediately. |

Child overlays such as menus and tooltips are not clipped automatically.


---

[Table of contents](../index_en.md) - Data | Previous: [TreeTable](06_treetable_en.md) | Next: [Button](../10-overlays/01_button_en.md) | [Русский](07_scrollbox_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
