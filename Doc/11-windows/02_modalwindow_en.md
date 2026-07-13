![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — core `0.6.7` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Windows | Previous: [UiWindow](01_uiwindow_en.md) | Next: [MessageWindow](03_messagewindow_en.md) | [Русский](02_modalwindow_ru.md)

---

# ModalWindow

A complete window with title bar, optional hamburger menu, close button, scrollable body, and footer buttons.

`RimUI.Adapter.ModalWindow : UiWindow`

## Example

```csharp
var body = new Field { Style = { Gap = 10f } };
body.Add(new Text("Description...") { Style = { Text = new TextStyle { Wrap = true } } });

var modal = new ModalWindow(body) {
    Title = "Modal example",
    FixedSize = new Vector2(460f, 300f),
    MenuItems = new List<MenuItem> { new MenuItem("Item 1", () => { }) },
};
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Title` | `string` | `null` | Raw title text. |
| `TitleText` | `Text` | `null` | Prebuilt title, preferred over `Title`. |
| `MenuItems` | `List<MenuItem>` | `null` | Hamburger menu items; null hides it. |
| `HamburgerIcon` | `string` | `Icons.Hamburger` | Custom menu icon key. |
| `CloseIcon` | `string` | `Icons.Close` | Custom close icon key. |
| `Body` | `UiElement` | constructor input | Body content. |
| `FooterButtons` | `List<UiElement>` (readonly) | empty | Footer buttons. |
| `FooterAlign` | `JustifyContent` | `End` | Footer alignment. |
| `Draggable` | `bool` | `true` | Enables custom title-bar dragging. |
| `OnClose` | `Action` | `null` | Called for every close path. |
| `PanelStyle` | `Style` | `Theme.ModalPanel` | Window panel style. |
| `SectionGap` | `float` | theme | Gap between header, body, and footer. |

### Exact title-bar dragging

Native RimWorld dragging is disabled. Pressing the left button in the header starts dragging immediately with no pixel threshold. The position is clamped on both axes so no edge can leave the screen.

## Styles

Panel slot: `Theme.ModalPanel`. A panel sprite disables the native window background and shadow.

## Methods

| Method | Returns | Description |
|---|---|---|
| `ModalWindow(UiElement body = null, Theme theme = null)` | - | Creates a modal. |
| `InitialSize` | `Vector2` | Automatic size or `FixedSize`. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnClose` | `Action` | - | Close button, window close control, or Escape. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `PanelStyle` and `Theme.ModalPanel`. |
| Sprite frame | Yes | Disables native background and shadow. |
| Animation | Yes | On child elements through `Style.Animation`. |
| Disabled | Not applicable | Footer controls have their own state. |
| Hover fade | Not on window | Available on child controls. |
| Dragging | Yes | Custom title-bar implementation. |


---

[Table of contents](../index_en.md) - Windows | Previous: [UiWindow](01_uiwindow_en.md) | Next: [MessageWindow](03_messagewindow_en.md) | [Русский](02_modalwindow_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
