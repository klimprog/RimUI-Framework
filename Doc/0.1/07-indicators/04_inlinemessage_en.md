![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Indicators | Previous: [Badge](03_badge_en.md) | Next: [MeterGroup](05_metergroup_en.md) | [Русский](04_inlinemessage_ru.md)

---

# InlineMessage

An in-flow notification banner rather than a popup. `Severity` controls its color and icon, and it may include a close button.

`RimUI.Components.InlineMessage : UiElement`

## Example

```csharp
new InlineMessage(Severity.Info, "Information");
new InlineMessage(Severity.Warn, "Warning") { Key = "im1", Closable = true };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Content` | `string` | `null` | Message text. |
| `TextElement` | `Text` | `null` | Prebuilt text element used instead of `Content`. |
| `Severity` | `Severity` | set by constructor | Controls icon and color; see `Tag` for the full mapping. |
| `Closable` | `bool` | `false` | Shows a close button. |
| `OnClose` | `Action` | `null` | Called after the banner closes. |

## Styles

Theme slot: `message` for gap, padding, radius, and border width. Background and border colors are derived from `Severity` inside the component and are not merged with user `Style`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `InlineMessage(Severity severity, string content = null)` (constructor) | - | Creates a severity banner with text. |
| `SetStyle(string path, string value)` | - | Applies the path to the root style; there are no named style parts. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnClose` | `Action` | - | After the banner is marked closed. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered. Keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |

`Events.Click` covers the entire banner and is independent of `OnClose`.

**Important:** closed state is stored by `Key` in the ID store. Give every simultaneously displayed message a unique `Key`, or closing one will close the others.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Partly | `message` supplies layout values; colors come from `Severity`. |
| Sprite frame | Yes | `slot.Sprite` from `message`. |
| Animation | Yes | Shared `Style.Animation` system. |
| Disabled | No | No disabled state of its own. |
| Hover fade | No | No smooth hover transition. |


---

[Table of contents](../index_en.md) - Indicators | Previous: [Badge](03_badge_en.md) | Next: [MeterGroup](05_metergroup_en.md) | [Русский](04_inlinemessage_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
