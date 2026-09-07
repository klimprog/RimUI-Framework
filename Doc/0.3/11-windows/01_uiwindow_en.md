![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.3.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Windows | Previous: [ConfirmPopup](../10-overlays/10_confirmpopup_en.md) | Next: [ModalWindow](02_modalwindow_en.md) | [Русский](01_uiwindow_ru.md)

---

# UiWindow

Base window class that hosts a framework element tree inside a RimWorld `Verse.Window`. Most code should use `ModalWindow` or its descendants.

`RimUI.Adapter.UiWindow : Verse.Window`

## Example

```csharp
var root = new Grid();
root.Cell(12, Button.Make("Hello"));
var window = new UiWindow(root) { FixedSize = new Vector2(400f, 300f) };
window.Show();
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Root` | `UiElement` | constructor input | Root content element. |
| `Theme` | `Theme` | `null` | Explicit window theme; null follows the active theme live. |
| `FixedSize` | `Vector2` | `Vector2.zero` | Fixed size, or zero for content sizing up to screen limits. |
| `Resizable` | `ResizeEdges` | `None` | Which edges the window can be dragged by. See "Resizing a window" below. |
| `MinWindowSize` | `Vector2` | `(180, 120)` | Lower bound of the size while resizing. |
| `MaxWindowSize` | `Vector2` | `Vector2.zero` | Upper bound; zero on an axis means the screen is the only limit. |

Window state, including scroll, focus, expansion, and internally stored control values, lives only from open to close. Reopening even the same instance starts clean. Persist data yourself through callbacks or `OnClose`.

## Exact automatic sizing

```text
maxW = UI.screenWidth * 0.92
maxH = UI.screenHeight * Theme.MaxWindowHeightFraction

width  = min(content.Width  + 36, maxW)
height = min(content.Height + 36, maxH)
```

The 36 px is two 18 px chrome margins. `maxH` uses full screen height without subtracting RimWorld UI. Content measures against `maxW - 36`. Internal scrolling starts when `content.Height > availableHeight + 0.5`.

## Resizing a window

```csharp
new ModalWindow(body)
{
    Title = "Report",
    Resizable = ResizeEdges.All,
    MinWindowSize = new Vector2(260f, 160f),
}.Show();
```

The `ResizeEdges` values are the same as for blocks (`All`, `Horizontal`, `Corners`, individual
sides and corners); the full list is on the "Style - the style model" page.

What is specific to windows:

* **dragging the left or top edge moves the position as well** — the opposite side stays put and
  the window does not creep away;
* **the dragged size is remembered** and returned instead of the computed one: a window with
  automatic sizing recomputes its size from the content every time the game asks for it (opening,
  a resolution change, a UI scale change), and without this the window would jump back;
* the grab zone sits **on the window's visible frame** — the one its panel draws, not on the
  border of the system rectangle: when the framework draws the panel, the native backdrop is off
  and the frame lies inside the margins. The zone is 3 points outwards and 6 inwards; both numbers
  are overridable (`ResizeGrabOut`, `ResizeGrab`).

## Dragging a window off-screen

A window can be dragged past an edge: up to **80%** of it goes past the left, right and bottom
edges, leaving a fifth of it on screen. Past the **top edge it does not go at all** — that is where
the header you drag it by lives, and once the header is gone there would be nothing left to grab.

The fraction is configurable by overriding `OffscreenFraction`.

The clamping is recomputed not only while dragging but also when the **screen resolution or the UI
scale changes**: otherwise a window dragged off-screen could end up entirely outside the border.

## Styles

Window panel: `Theme.WindowPanelFor(SheetKey)`. The default protected `SheetKey` is `"window"`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `UiWindow(UiElement root, Theme theme = null)` | - | Creates a window. |
| `Show()` | `void` | Adds it to `Find.WindowStack`. |
| `InitialSize` | `Vector2` | Automatic size or `FixedSize`. |

## Events

Lifecycle uses overridable `Verse.Window` methods. `ModalWindow` adds `OnClose`.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Panel style | Yes | `Theme.WindowPanelFor(SheetKey)`. |
| Sprite frame | Yes | A panel sprite disables the native window background. |
| Overflow scrolling | Yes | Internal framework `ScrollBox`. |
| State across opens | No | Each opening starts clean. |
| Disabled / hover fade | Not applicable | Belongs to child elements. |


---

[Table of contents](../index_en.md) - Windows | Previous: [ConfirmPopup](../10-overlays/10_confirmpopup_en.md) | Next: [ModalWindow](02_modalwindow_en.md) | [Русский](01_uiwindow_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
