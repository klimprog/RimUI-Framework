![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.3.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Windows | Previous: [ModalWindow](02_modalwindow_en.md) | Next: [ConfirmWindow](04_confirmwindow_en.md) | [Русский](03_messagewindow_ru.md)

---

# MessageWindow

A simplified modal without a hamburger menu and with one footer button that closes it.

`RimUI.Adapter.MessageWindow : ModalWindow`

## Example

```csharp
MessageWindow.OfText("Information", "Message text...").Show();
```

## Parameters

Adds no public fields; it inherits `Title`, `Body`, `FooterButtons`, `OnClose`, `PanelStyle`, and `Draggable` from `ModalWindow`.

| Parameter | Type | Default | Description |
|---|---|---|---|
| - | - | - | See `ModalWindow`. |

## Styles

Uses its own `Theme.MessageboxPanel` slot with `SheetKey = "messagebox"`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `MessageWindow(string title, UiElement body, string buttonText = "OK", Button button = null, Theme theme = null)` | - | Creates a message window. |
| `static OfText(string title, string text, string buttonText = "OK", float width = 340f)` | `MessageWindow` | Creates wrapped text content. |

## Events

Inherited `OnClose` runs for every close path, including the footer button.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `Theme.MessageboxPanel` independently of `Theme.ModalPanel`. |
| Sprite frame | Yes | Separate from regular modals. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled / hover fade | See `ModalWindow` | Fully inherited. |


---

[Table of contents](../index_en.md) - Windows | Previous: [ModalWindow](02_modalwindow_en.md) | Next: [ConfirmWindow](04_confirmwindow_en.md) | [Русский](03_messagewindow_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
