![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [Button](01_button_en.md) | Next: [DropdownMenu](03_dropdownmenu_en.md) | [Русский](02_popover_ru.md)

---

# Popover

An anchored popup panel, usually attached to a button. Clicking the trigger toggles it; clicking outside closes it. Content may be any element.

`RimUI.Elements.Popover : UiElement` (sealed)

## Example

```csharp
var pop = new Popover { Trigger = Button.Make("Text v") };
pop.Content = new Text("Popup content") { Style = { Text = new TextStyle { Wrap = true } } };

var pop2 = new Popover { Trigger = Button.Make("Panel"), Placement = OverlayPlacement.Right };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Trigger` | `UiElement` | `null` | In-flow anchor and toggle area. |
| `Content` | `UiElement` | `null` | Panel content. |
| `Placement` | `OverlayPlacement` | `Bottom` | `Bottom`, `Top`, `Right`, or `Left`. |
| `Gap` | `float` | `4` | Gap from the anchor. |

## Styles

`Theme.PopoverPanel` supplies panel defaults. Border, background, radius, padding, and shadow are supported.

## Methods

Only object-initializer construction is exposed.

| Method | Returns | Description |
|---|---|---|
| `SetStyle(string path, string value)` | - | Applies the path to the root style. Invalid paths are ignored. |

## Events

Open state lives in the ID store; there is no open/close callback.

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the element bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Never from `Popover` itself; it has no disabled state. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `Theme.PopoverPanel`. |
| Sprite frame | Yes | `ps.Sprite` through `SpriteBox`. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled | No | Depends on the trigger. |
| Hover fade | No | Opens by click. |

The panel renders in the overlay layer, is clamped to the window, and may flip sides.

## Exact placement and flipping

On the cross axis, the panel aligns to the anchor start (`anchor.X` or `anchor.Y`), not its center. It flips only when the requested side does not fit and the opposite side does. For example, Bottom becomes Top when `anchor.Bottom + gap + h > area.Bottom` and `anchor.Y - gap - h >= area.Y`. Right/Left is symmetric.

The test is strict, with no pixel margin. If neither side fits, no flip occurs; the original side is then clamped to the window. Right/bottom clamping runs first, then left/top, so a panel larger than the area aligns to the top-left.


---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [Button](01_button_en.md) | Next: [DropdownMenu](03_dropdownmenu_en.md) | [Русский](02_popover_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
