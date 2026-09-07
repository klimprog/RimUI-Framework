![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [Popover](02_popover_en.md) | Next: [Menu](04_menu_en.md) | [Русский](03_dropdownmenu_ru.md)

---

# DropdownMenu

A button-anchored menu with disabled items and nested submenus that open on hover.

`RimUI.Elements.DropdownMenu : UiElement` (sealed)

## Example

```csharp
var menu = new DropdownMenu { Trigger = Button.Make("Menu") };
menu.AddItem(new MenuItem("Create", () => { }));
menu.AddItem(new MenuItem("Unavailable") { Disabled = true });
menu.AddItem(new MenuItem("Export")
    .Sub(new MenuItem("As PNG", () => { }))
    .Sub(new MenuItem("As JSON", () => { })));
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `Trigger` | `UiElement` | `null` | Anchor that opens the menu. |
| `Items` | `List<MenuItem>` (readonly) | empty | Top-level items. |
| `ItemTextStyle` | `TextStyle` | theme | Item text style. |
| `ItemHoverColor` | `ColorRGBA?` | `null` | Item hover color. |
| `DisabledTextColor` | `ColorRGBA?` | `null` | Disabled text color. |
| `ItemHeight` | `float` | `0` (theme) | Item height. |
| `IconSlot` | `float` | `0` (theme) | Icon-column width when any item has an icon. |
| `MinWidth` | `float` | `0` (theme) | Minimum panel width. |

`MenuItem` exposes text, icon, callback, disabled state, and child items. `Sub` adds a child and returns the parent. `HasChildren` reports nested items.

### Exact placement

The root opens below with a 4 px gap and `Popover` flipping. A submenu opens flush to the right of its parent row, not the whole panel, and may flip left. Its top aligns with the parent row and it opens on hover.

Width fits the widest item, including icon and chevron slots, but is at least `MinWidth` or `Theme.MenuMinWidth = 140`. Height is item heights plus gaps and panel padding.

## Styles

Panel slot: `Theme.MenuPanel`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `AddItem(MenuItem item)` | `DropdownMenu` | Adds an item and returns this menu. |
| `OpenAt(UiState s, Vec2 pos)` | `void` | Opens at an arbitrary point without requiring `Trigger`; used by `ContextMenu`. |
| `SetStyle(string path, string value)` | - | Applies the path to the root style. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `MenuItem.OnClick` | `Action` | - | A leaf item is clicked. Items with children open their submenu instead. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the element bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Not raised by the menu itself; disabled state belongs to items. |

Universal click events cover the menu component, not individual items.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `Theme.MenuPanel`. |
| Sprite frame | Yes | Panel background supports sprite styling. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled | Yes | Per `MenuItem.Disabled`. |
| Hover fade | Yes | Item rows use IDs based on `depth * 64 + i`. |


---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [Popover](02_popover_en.md) | Next: [Menu](04_menu_en.md) | [Русский](03_dropdownmenu_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
