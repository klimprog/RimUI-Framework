![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.15` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [Menu](04_menu_en.md) | Next: [TooltipBox](06_tooltipbox_en.md) | [Русский](05_contextmenu_ru.md)

---

# ContextMenu

A context menu opened by right-clicking an arbitrary target.

`RimUI.Components.ContextMenu : UiElement` (sealed)

## Example

```csharp
var target = new Field();
target.Add(new Text("Right-click here"));
var cm = new ContextMenu(target);
cm.AddItem(new MenuItem("Action", () => { }));
cm.AddItem(new MenuItem("Submenu").Sub(new MenuItem("Nested", () => { })));
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Target` | `UiElement` | constructor input | Right-click target. |
| `Panel` | `DropdownMenu` (readonly) | new `DropdownMenu()` | Tiered menu panel. |

## Styles

Uses `DropdownMenu` styling through `Panel`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `ContextMenu(UiElement target = null)` (constructor) | - | Creates a menu for a target. |
| `AddItem(MenuItem item)` | `ContextMenu` | Adds through `Panel.AddItem` and returns this menu. |
| `SetStyle(string path, string value)` | - | Applies to the `ContextMenu` root, not `Panel`. |

After `Arrange`, component bounds match `Target`. Right-click calls `Panel.OpenAt(s, s.MousePosition)`.

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `MenuItem.OnClick` | `Action` | - | A panel item is clicked. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Not raised by `ContextMenu` itself. |

`Events.RightClick` runs after `Panel.OpenAt` on the same click. Universal events observe completed component logic; they do not replace it.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Internal `DropdownMenu`. |
| Sprite frame | Yes | Same as `DropdownMenu`. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled | Yes | Per menu item. |
| Hover fade | Yes | On menu rows. |


---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [Menu](04_menu_en.md) | Next: [TooltipBox](06_tooltipbox_en.md) | [Русский](05_contextmenu_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
