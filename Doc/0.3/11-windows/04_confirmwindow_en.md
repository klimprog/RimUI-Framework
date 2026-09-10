![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.31` · mod `0.3.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Windows | Previous: [MessageWindow](03_messagewindow_en.md) | Next: [ImageBox](../12-display/01_imagebox_en.md) | [Русский](04_confirmwindow_ru.md)

---

# ConfirmWindow

A two-button confirmation window with separate accept and decline callbacks. **Every close path other than explicit acceptance counts as decline**, including Escape and the close button.

`RimUI.Adapter.ConfirmWindow : ModalWindow`

## Example

```csharp
ConfirmWindow.OfText("Delete node?", "This cannot be undone.",
    onAccept: () => DeleteBlock(node),
    onDecline: null).Show();
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `OnAccept` | `Action` | `null` | Called after acceptance and closing. |
| `OnDecline` | `Action` | `null` | Called for cancel, Escape, or window close. |

Other fields are inherited from `ModalWindow`.

## Styles

Uses its own `Theme.ConfirmPanel` slot with `SheetKey = "confirm"`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `ConfirmWindow(string title, UiElement body, string acceptText = "Yes", string declineText = "Cancel", Button accept = null, Button decline = null, Theme theme = null)` | - | Creates a confirmation window. |
| `static OfText(string title, string text, Action onAccept, Action onDecline = null, float width = 340f)` | `ConfirmWindow` | Creates one with wrapped text. |

The accept button defaults to `Success` and decline to `Danger`.

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnAccept` | `Action` | - | The accept button is clicked. |
| `OnDecline` | `Action` | - | Any close except explicit acceptance. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `Theme.ConfirmPanel` independently of `Theme.ModalPanel`. |
| Sprite frame | Yes | Separate from regular modals. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled / hover fade | See `ModalWindow` | Fully inherited. |


---

[Table of contents](../index_en.md) - Windows | Previous: [MessageWindow](03_messagewindow_en.md) | Next: [ImageBox](../12-display/01_imagebox_en.md) | [Русский](04_confirmwindow_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
