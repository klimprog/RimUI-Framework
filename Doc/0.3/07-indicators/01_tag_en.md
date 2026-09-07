![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.3.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Indicators | Previous: [ColorPicker](../06-selection/07_colorpicker_en.md) | Next: [Chip](02_chip_en.md) | [Русский](01_tag_ru.md)

---

# Tag

A non-interactive colored status label. `Severity` controls its color, and an icon can be added. The component itself does not handle clicks.

`RimUI.Components.Tag : FlexBox`

## Example

```csharp
Row(new Tag("NEW", Severity.Success), new Tag("BETA", Severity.Warn),
    new Tag("BROKEN", Severity.Error), new Tag("INFO") { IconIndex = Icons.Info });
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Content` | `string` | `null` | Label text. |
| `TextElement` | `Text` | `null` | Prebuilt text element used instead of `Content`. |
| `IconIndex` | `int` | `-1` | Index in the `Icons` sheet. |
| `Severity` | `Severity` | `Info` | Controls the background color. |
| `Rounded` | `bool` | `true` | Uses pill radius (`BorderRadius.Middle`); `false` uses `Small`. |

`RimUI.Components.Severity` is shared by `Tag`, `Badge`, and `InlineMessage`:

| Value | Background |
|---|---|
| `Info` | Theme accent (`Theme.Accent`) |
| `Success` | Green (`Theme.ButtonSuccessBg`) |
| `Warn` | Yellow (`Theme.ButtonWarningBg`) |
| `Error` | Red (`Theme.ButtonDangerBg`) |

## Styles

Theme slot: `tag` for gap, padding, and radius.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Tag(string content = null, Severity severity = Severity.Info)` (constructor) | - | Creates a tag with text and severity. |
| `SetStyle(string path, string value)` | - | Applies a path such as `text.color` to the root style; `Tag` has no named style parts. |

## Events

There are no custom callbacks. Universal events remain available even though `Tag` is not interactive:

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered. Keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `tag` slot. |
| Sprite frame | Yes | `slot.Sprite` from `tag`. |
| Animation | Yes | Works through `Animated.Wrap` and `Style.Animation`. |
| Disabled | No | Non-interactive element. |
| Hover fade | No | No smooth hover transition. |


---

[Table of contents](../index_en.md) - Indicators | Previous: [ColorPicker](../06-selection/07_colorpicker_en.md) | Next: [Chip](02_chip_en.md) | [Русский](01_tag_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
