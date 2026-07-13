![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — core `0.6.7` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [DropdownMenu](03_dropdownmenu_en.md) | Next: [ContextMenu](05_contextmenu_en.md) | [Русский](04_menu_ru.md)

---

# Menu

A menu that can be embedded in layout flow or shown as an overlay. `Popup` switches between the modes.

`RimUI.Components.Menu : UiElement` (sealed)

## Example

```csharp
var m = new Menu { Style = { Width = 220f } };
m.AddHeader("File");
m.AddItem(new MenuItem("Create", () => { }));
m.AddSeparator();
m.AddItem(new MenuItem("Unavailable") { Disabled = true });
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Trigger` | `UiElement` | `null` | Anchor used only in popup mode. |
| `Popup` | `bool` | `false` | Embeds a flat list when false; reuses `DropdownMenu` as an overlay when true. |
| `ItemHeight` | `float` | `26` | Item height. |
| `HeaderHeight` | `float` | `20` | Group-header height. |
| `SeparatorHeight` | `float` | `9` | Separator height. |
| `MinWidth` | `float` | `160` | Minimum width. |

Uses the same `MenuItem` as `DropdownMenu`.

## Styles

Panel slot: `Theme.MenuPanel`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `AddItem(MenuItem item)` | `Menu` | Adds an item and returns this menu. |
| `AddHeader(string text)` | `Menu` | Adds a group header. |
| `AddSeparator()` | `Menu` | Adds a separator. |
| `SetStyle(string path, string value)` | - | Applies the path to the root style. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `MenuItem.OnClick` | `Action` | - | A leaf item is clicked. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Not raised by `Menu` itself; disabled state belongs to items. |

Universal clicks cover the menu rather than individual items.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `Theme.MenuPanel`. |
| Sprite frame | Yes | Standard panel background drawing. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled | Yes | Per `MenuItem.Disabled`. |
| Hover fade | No (static mode) | Static rows highlight immediately, unlike `DropdownMenu`. |


---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [DropdownMenu](03_dropdownmenu_en.md) | Next: [ContextMenu](05_contextmenu_en.md) | [Русский](04_menu_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
