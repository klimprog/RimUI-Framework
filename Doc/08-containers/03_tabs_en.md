![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — core `0.6.7` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Containers | Previous: [Accordion](02_accordion_en.md) | Next: [ListBox&lt;T&gt;](../09-data/01_listbox_en.md) | [Русский](03_tabs_ru.md)

---

# Tabs

Tabbed content with headers on the top, bottom, left, or right. Clicking a header changes the active page, whose tab visually joins the body.

`RimUI.Components.Tabs : UiElement`

## Example

```csharp
var tb = new Tabs { Position = TabPosition.Top };
tb.Tab("Weapons", weaponsBody);
tb.Tab("Armor", armorBody);
tb.Tab("Off", disabledBody, disabled: true);
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Pages` | `List<TabPage>` (readonly) | empty | Tab pages. |
| `DefaultTab` | `int` | `0` | Initially active index. |
| `TabHeight` | `float` | `30` | Tab header height. |
| `TabPadding` | `float` | `14` | Horizontal header padding. |
| `Position` | `TabPosition` (`Top`, `Bottom`, `Left`, `Right`) | `Top` | Header side. |

`TabPage` contains `string Title`, `UiElement Body`, and `bool Disabled`. Disabled tabs use a separate text color and cannot be clicked.

## Exact sizing and overflow

Each tab width is `textWidth(Title) + 3 + TabPadding * 2`. Top and bottom layouts use each tab's own width. Left and right layouts use one shared width equal to the widest tab.

Overflow is not handled: tabs may extend outside the container. There is no wrapping, horizontal scrolling, or text truncation. Left and right tab labels remain horizontal; the active strip becomes a 2 px vertical line against the body edge.

## Styles

Slots: `tabs/tab`, `tabs/tab_active`, `tabs/tab_hover`, `tabs/tab_text*`, `tabs/body`, and `tabs/strip`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Tab(string title, UiElement body, bool disabled = false)` | `Tabs` | Adds a tab and returns this component. |
| `GetSelected()` | `int` | Active index; before the first render, returns `DefaultTab`. |
| `SetSelected(int index)` | - | Selects a tab and calls `OnChange`. Applied on the next `Measure` pass because the active page affects body layout. |
| `SetStyle(string path, string value)` | - | Applies the path to the root style; headers, body, and strip use theme slots. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnChange` | `Action<int>` | new active index | Header click or `SetSelected`. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | A header click changes the active tab. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the component bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the component. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |

The universal click events cover the entire `Tabs` component, not an individual header.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `tabs/*` slots. |
| Sprite frame | Yes | Through tab background slots. |
| Animation | No custom animation | Selection is immediate. |
| Disabled | Yes | Per `TabPage.Disabled`. |
| Hover fade | Yes | Inactive tabs use `Theme.HoverFadeDuration`; active state is immediate. |


---

[Table of contents](../index_en.md) - Containers | Previous: [Accordion](02_accordion_en.md) | Next: [ListBox&lt;T&gt;](../09-data/01_listbox_en.md) | [Русский](03_tabs_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
