![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.3.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [Text](09_text_en.md) | Next: [UiWindow](../11-windows/01_uiwindow_en.md) | [Русский](10_confirmpopup_ru.md)

---

# ConfirmPopup

A compact confirmation right next to the element it belongs to: a message and two buttons, without
a full-screen modal window.

`RimUI.Components.ConfirmPopup : UiElement`

## Which one to use

| Situation | Component |
|---|---|
| The action is reversible and local: delete a row, reset a filter, clear a field | `ConfirmPopup` |
| The action is irreversible or important: delete a save, apply changes to the whole colony | `ConfirmWindow` |

`ConfirmPopup` is easy to miss — that is the point, it is meant not to interrupt. Which is exactly
why irreversible actions keep the modal window that cannot be overlooked.

## Example

```csharp
var deleteBtn = new Button { Content = new Text("Delete"), Preset = ButtonPreset.Danger };

new ConfirmPopup
{
    Trigger = deleteBtn,
    Message = "Delete the selected record? This cannot be undone.",
    AcceptLabel = "Yes",
    RejectLabel = "No",
    Danger = true,
    OnAccept = () => rows.RemoveAt(index)
};

// confirmation to the right of the target, with a different icon
new ConfirmPopup
{
    Trigger = resetBtn,
    Message = "Reset the filters?",
    Icon = Icons.Info,
    Placement = OverlayPlacement.Right,
    OnAccept = ResetFilters
};
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `Trigger` | `UiElement` | `null` | The target element: clicking it opens the confirmation. |
| `Message` | `string` | `""` | The question. Wraps by words. |
| `Icon` | `int` | `Icons.Warning` | Icon to the left of the message (`-1` = none). |
| `AcceptLabel` | `string` | `"OK"` | Caption of the confirm button. |
| `RejectLabel` | `string` | `"X"` | Caption of the reject button. |
| `OnAccept` | `Action` | `null` | Confirmed. |
| `OnReject` | `Action` | `null` | Rejected (the "no" button). |
| `Placement` | `OverlayPlacement` | `Bottom` | Which side it opens on: `Bottom`, `Top`, `Right`, `Left`. |
| `Danger` | `bool` | `false` | The confirm button is red (a destructive action). |
| `MaxMessageWidth` | `float` | `260` | The width the message wraps at. |
| `Disabled` | `bool` | `false` | Disabled state. |

## Behaviour

**Built on `Popover`.** The overlay layer, the placement next to the anchor and the close-on-click-
outside are solved there: the panel is not clipped by its parent, it is kept inside the window, and
if there is not enough room on the requested side it opens on the opposite one.

**Closing.** The panel closes on either button, on a click outside, on a second click on the
target, or programmatically through `Close()`.

**Clicking outside does not raise `OnReject`.** `OnReject` is a deliberate "no" — a button press;
clicking outside means "changed my mind about answering" and should not run reject logic.

**Button order.** Reject on the left, confirm on the right, so the confirmation ends up closer to
the edge the hand naturally moves to after reading the text.

## Styles

The panel is a `Popover`, so it is themed through the `PopoverPanel` slot (background, border,
radius, padding, shadow). The buttons are ordinary `Button`s using the `button` slot; with `Danger`
the confirm button takes its colour from the `ButtonPreset.Danger` preset.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Close()` | — | Close the confirmation programmatically — for instance once the action has completed asynchronously. The panel closes on the next frame. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnAccept` | `Action` | — | The confirm button was pressed. |
| `OnReject` | `Action` | — | The reject button was pressed (but not a click outside). |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The cursor entered or left the bounds of the **target** (the panel is out of flow). |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left / right click on the target. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | `Disabled` changed. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Styling | Yes | Through the `PopoverPanel` slot and the button styles. |
| Sprite frame | Yes | The panel supports 9-slice like any `Popover`. |
| Animation | Yes | The common `Style.Animation` mechanism. |
| Disabled | Yes | The buttons inside the panel are blocked. |
| Hover fade | Yes | The buttons inside get the usual smooth button hover. |
| Cursor | Yes | A hand over the target and the buttons. |


---

[Table of contents](../index_en.md) - Overlays and menus | Previous: [Text](09_text_en.md) | Next: [UiWindow](../11-windows/01_uiwindow_en.md) | [Русский](10_confirmpopup_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
