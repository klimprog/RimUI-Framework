![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [ContextMenu](05_contextmenu_en.md) | Next: [Icon](07_icon_en.md) | [Русский](06_tooltipbox_ru.md)

---

# TooltipBox

A tooltip template whose content may be any element, unlike RimWorld's text-only native tooltip. It follows the pointer and does not intercept clicks.

`RimUI.Elements.TooltipBox : UiElement` (sealed)

## Example

```csharp
var body = new Field { Style = { Gap = 6f } };
body.Add(new Text("Tooltip title"));
body.Add(new Divider());
var tip = new TooltipBox { Target = Button.Make("Hover me"), Content = body };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Target` | `UiElement` | `null` | In-flow hover target. |
| `Content` | `UiElement` | `null` | Any tooltip content. |
| `CursorOffset` | `Vec2` | `(14, 20)` | Panel offset from the pointer. |

`CursorOffset` moves the panel's top-left corner 14 px right and 20 px below the pointer hotspot. The panel clamps flush to window edges using the same boundary logic as `Popover`.

## Styles

Panel slot: `Theme.TooltipPanel`.

## Methods

Only object-initializer construction is exposed.

| Method | Returns | Description |
|---|---|---|
| `SetStyle(string path, string value)` | - | Applies the path to the root style. |

## Events

Visibility follows hover; there is no show/hide callback.

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Never from `TooltipBox` itself. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `Theme.TooltipPanel`. |
| Sprite frame | Yes | `ps.Sprite` through `SpriteBox`. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled | No | No disabled state. |
| Hover fade | No | Appears and disappears immediately. |

The panel closes when hover ends and is not registered as an overlay rectangle, so clicks and hover pass through it.


---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [ContextMenu](05_contextmenu_en.md) | Next: [Icon](07_icon_en.md) | [Русский](06_tooltipbox_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
