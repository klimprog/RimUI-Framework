![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.15` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Indicators | Previous: [SegmentProgress](09_segmentprogress_en.md) | Next: [Panel](../08-containers/01_panel_en.md) | [Русский](10_divider_ru.md)

---

# Divider

A thin horizontal or vertical separator.

`RimUI.Elements.Divider : UiElement`

## Example

```csharp
container.Add(new Divider());
var vDiv = new Divider(vertical: true, thickness: 2f, color: Orange);
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Vertical` | `bool` | `false` | Uses vertical orientation. |
| `Thickness` | `float` | `1` | Line thickness. |

Color is the constructor's third argument. When omitted, it comes from `Theme.DividerStyle`.

## Styles

Theme slot: `Theme.DividerStyle`. An explicit constructor color directly sets `Style.Background = Fill.Solid(color)`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Divider(bool vertical = false, float thickness = 1f, ColorRGBA? color = null)` (constructor) | - | Creates a divider. |
| `SetStyle(string path, string value)` | - | Applies a path such as `color` to the root style. |

## Events

No custom callbacks; universal events are available:

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered. Keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Uses normal resolved style. |
| Sprite frame | Yes | `Style.Sprite` works through base `UiElement.Emit`, although the theme supplies only a color. |
| Animation | Yes | Shared `Style.Animation` system. |
| Disabled | No | Non-interactive element. |
| Hover fade | No | No smooth hover transition. |


---

[Table of contents](../index_en.md) - Indicators | Previous: [SegmentProgress](09_segmentprogress_en.md) | Next: [Panel](../08-containers/01_panel_en.md) | [Русский](10_divider_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
