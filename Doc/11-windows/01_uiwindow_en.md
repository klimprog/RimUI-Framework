![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — core `0.6.7` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Windows | Previous: [Text](../10-overlays/09_text_en.md) | Next: [ModalWindow](02_modalwindow_en.md) | [Русский](01_uiwindow_ru.md)

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

Window state, including scroll, focus, expansion, and internally stored control values, lives only from open to close. Reopening even the same instance starts clean. Persist data yourself through callbacks or `OnClose`.

## Exact automatic sizing

```text
maxW = UI.screenWidth * 0.92
maxH = UI.screenHeight * Theme.MaxWindowHeightFraction

width  = min(content.Width  + 36, maxW)
height = min(content.Height + 36, maxH)
```

The 36 px is two 18 px chrome margins. `maxH` uses full screen height without subtracting RimWorld UI. Content measures against `maxW - 36`. Internal scrolling starts when `content.Height > availableHeight + 0.5`.

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

[Table of contents](../index_en.md) - Windows | Previous: [Text](../10-overlays/09_text_en.md) | Next: [ModalWindow](02_modalwindow_en.md) | [Русский](01_uiwindow_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
