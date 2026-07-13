![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — core `0.6.7` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Indicators | Previous: [Chip](02_chip_en.md) | Next: [InlineMessage](04_inlinemessage_en.md) | [Русский](03_badge_ru.md)

---

# Badge

A circular counter drawn over another element. It displays text or, when empty, a simple dot such as an unread indicator.

`RimUI.Components.Badge : UiElement`

## Example

```csharp
Row(new Badge(() => "7"),
    new Badge(() => "99+") { Severity = Severity.Warn },
    new Badge { Severity = Severity.Error },
    new Badge(() => "3") { Target = Button.Make("With badge") });
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Value` | `Func<string>` | `null` | Badge text. `null` or empty produces a `DotSize` dot. |
| `Severity` | `Severity` | `Error` | Color level: accent, green, yellow, or red; see `Tag`. |
| `Target` | `UiElement` | `null` | Draws the badge over the target's top-right corner. |
| `MinSize` | `float` | `16` | Minimum circle size. |
| `DotSize` | `float` | `8` | Size when no text is shown. |

## Styles

Theme slots: `badge` for the background and `badge/text` for text.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Badge(Func<string> value = null)` (constructor) | - | Creates a badge with a text source. |
| `static Count(Func<int> count)` | `Badge` | Wraps a numeric source as a text badge. |
| `SetStyle(string path, string value)` | - | Applies the path to the root style; `Badge` has no named style parts. |

## Events

No custom callbacks; universal events are available:

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
| Style | Partly | Placement ignores the element's resolved style, while background and text come from slots. |
| Sprite frame | Yes, special | The `badge` slot is drawn as one `DrawCommand.Image`, not as 9-slice. |
| Animation | Yes | Shared `Style.Animation` system. |
| Disabled | No | Non-interactive element. |
| Hover fade | No | No smooth hover transition. |


---

[Table of contents](../index_en.md) - Indicators | Previous: [Chip](02_chip_en.md) | Next: [InlineMessage](04_inlinemessage_en.md) | [Русский](03_badge_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
