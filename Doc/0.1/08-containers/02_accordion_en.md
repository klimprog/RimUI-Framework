![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Containers | Previous: [Panel](01_panel_en.md) | Next: [Tabs](03_tabs_en.md) | [Русский](02_accordion_ru.md)

---

# Accordion

A list of title-and-body sections. Clicking a header toggles its section. With `Multiple = false`, opening one closes the others.

`RimUI.Components.Accordion : UiElement`

## Example

```csharp
var acc = new Accordion();
acc.Section("General", body1, startOpen: true);
acc.Section("Combat", body2);
acc.Section("Misc", body3);
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Sections` | `List<AccordionSection>` (readonly) | empty | Sections. |
| `Multiple` | `bool` | `false` | Allows several sections to stay open. |
| `HeaderHeight` | `float` | `30` | Section header height. |
| `Gap` | `float` | `4` | Gap between sections. |

`AccordionSection` contains `string Title` or `Text TitleElement`, `UiElement Body`, and `bool StartOpen`.

Expansion is immediate like `Panel`: the boolean state changes on click and `Measure` uses it directly, without height interpolation.

## Styles

Slots: `accordion/header`, `accordion/header_hover`, `accordion/body`, `accordion/title`, and `accordion/chevron`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Section(string title, UiElement body, bool startOpen = false)` | `Accordion` | Adds a section and returns this accordion. |
| `SetStyle(string path, string value)` | - | Applies the path to the root style. Section headers and bodies use theme slots. |

## Events

There is no `OnChange` or `OnToggle`; expansion is stored in the ID store.

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the component bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the component. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |

`Events.Click` covers the entire accordion rather than a particular section.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `accordion/*` slots. |
| Sprite frame | Partly | Through each resolved `_headStyles[i].Sprite`. |
| Animation | No (except `Style.Animation`) | Expansion is discrete. |
| Disabled | No | No disabled sections. |
| Hover fade | No | Headers use immediate `HoverOn`. |


---

[Table of contents](../index_en.md) - Containers | Previous: [Panel](01_panel_en.md) | Next: [Tabs](03_tabs_en.md) | [Русский](02_accordion_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
