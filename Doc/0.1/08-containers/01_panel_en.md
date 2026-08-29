![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Containers | Previous: [Divider](../07-indicators/10_divider_en.md) | Next: [Accordion](02_accordion_en.md) | [Русский](01_panel_ru.md)

---

# Panel

A panel with a header and arbitrary body content. It can be collapsible: clicking the header hides or shows the body.

`RimUI.Components.Panel : UiElement`

## Example

```csharp
var p = new Panel("Title", myBody) { Collapsible = true };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Title` | `string` | `null` | Header text. |
| `TitleElement` | `Text` | `null` | Prebuilt title, preferred over `Title`. |
| `Body` | `UiElement` | `null` | Panel body. |
| `Collapsible` | `bool` | `false` | Enables header-click collapsing. |
| `StartCollapsed` | `bool` | `false` | Initial state when collapsible. |
| `HeaderHeight` | `float` | `30` | Header height. |

## Styles

Slots: `panel/frame` (9-slice), `panel/header`, `panel/header_hover`, `panel/title`, and `panel/chevron`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Panel(string title = null, UiElement body = null)` (constructor) | - | Creates a panel with a title and body. |
| `SetStyle(string path, string value)` | - | Applies the path to the root style. Header and frame styling comes from theme slots. |

## Events

Collapse state is stored as `BoolState` in the ID store and handled internally. No custom callback is exposed.

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the component bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the component. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |

`Events.Click` covers the whole panel, including the header, independently of collapse handling.

**Collapse is immediate.** Header clicks toggle the boolean state directly. `Measure` uses either header plus body height or header height alone; there is no height interpolation.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `Style` and `panel/*` slots. |
| Sprite frame | Yes | `panel/frame` (9-slice). |
| Animation | No (except `Style.Animation`) | Collapse is discrete. |
| Disabled | No | No disabled state. |
| Hover fade | No | Header hover changes immediately. |


---

[Table of contents](../index_en.md) - Containers | Previous: [Divider](../07-indicators/10_divider_en.md) | Next: [Accordion](02_accordion_en.md) | [Русский](01_panel_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
