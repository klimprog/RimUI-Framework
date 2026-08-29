![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.9` · mod `0.2.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Indicators | Previous: [Tag](01_tag_en.md) | Next: [Badge](03_badge_en.md) | [Русский](02_chip_ru.md)

---

# Chip

An interactive pill on a neutral background. It may include an icon and a close button that actually removes or deselects the chip.

`RimUI.Components.Chip : FlexBox`

## Example

```csharp
new Chip("Default");
new Chip("With icon") { IconIndex = Icons.Dot };
row.Add(new Chip(label, () => labels.Remove(label)));
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Content` | `string` | `null` | Chip text. |
| `TextElement` | `Text` | `null` | Prebuilt text element used instead of `Content`. |
| `IconIndex` | `int` | `-1` | Index in the `Icons` sheet. |
| `Removable` | `bool` | automatically `true` when `onRemove` is supplied | Shows the close button. |
| `OnRemove` | `Action` | `null` | Close-button callback. |

## Styles

Theme slot: `chip`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Chip(string content = null, Action onRemove = null)` (constructor) | - | Creates a chip and enables `Removable` when a callback is supplied. |
| `SetStyle(string path, string value)` | - | Applies a path such as `background` to the root style; `Chip` has no named style parts. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnRemove` | `Action` | - | The close button is clicked. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered. Keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |

`Events.Click` covers the whole pill and is independent of `OnRemove`.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `chip` slot. |
| Sprite frame | Yes | `slot.Sprite` from `chip`. |
| Animation | Yes | Shared `Style.Animation` system. |
| Disabled | No | No disabled state of its own. |
| Hover fade | No | No smooth hover transition. |


---

[Table of contents](../index_en.md) - Indicators | Previous: [Tag](01_tag_en.md) | Next: [Badge](03_badge_en.md) | [Русский](02_chip_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
