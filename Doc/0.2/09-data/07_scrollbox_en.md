![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.15` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Data | Previous: [TreeTable](06_treetable_en.md) | Next: [Paginator](08_paginator_en.md) | [Русский](07_scrollbox_ru.md)

---

# ScrollBox

Wraps one child in vertical scrolling when its content may exceed the available height.

`RimUI.Elements.ScrollBox : UiElement`

## Example

```csharp
var scroll = new ScrollBox(myTallContent) { Key = "myScroll" };
```

## Horizontal scrolling

The `Horizontal` flag enables scrolling sideways and is **off by default**. That is not caution for
its own sake: turning it on changes how the content is measured — instead of wrapping to the
available width, the container measures the content as infinitely wide. For a vertical list that
would mean text stops wrapping.

With both axes on, the bars do not overlap: the vertical one spans the height of the viewport, the
horizontal one its width, and the corner stays free.

The wheel: `Shift` switches the axis, as in browsers. If there is no vertical scrolling at all, the
wheel scrolls horizontally without `Shift` too — otherwise the input would simply be lost.

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `Child` | `UiElement` | `null` | Single contained element. |
| `ScrollbarWidth` | `float` | `10` | Scrollbar width. |
| `WheelStep` | `float` | `40` | Multiplier applied to wheel delta. |
| `Horizontal` | `bool` | `false` | Horizontal scrolling (see the section above). Opt-in: enabling it changes how content is measured. |
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

[Table of contents](../index_en.md) - Data | Previous: [TreeTable](06_treetable_en.md) | Next: [Paginator](08_paginator_en.md) | [Русский](07_scrollbox_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
