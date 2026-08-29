![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.9` · mod `0.2.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Indicators | Previous: [MeterGroup](05_metergroup_en.md) | Next: [ProgressSpinner](07_progressspinner_en.md) | [Русский](06_progressbar_ru.md)

---

# ProgressBar

A progress bar for values from 0 to 1, with an indeterminate mode for progress that cannot be measured in advance.

`RimUI.Components.ProgressBar : UiElement`

## Example

```csharp
var pb = new ProgressBar(() => (Time.realtimeSinceStartup * 0.1f) % 1f) { StretchFill = true };
pb.FillStyle.Background = Fill.Linear(GradientDirection.Horizontal,
    new GradientStop(0f, Green), new GradientStop(1f, Orange));

new ProgressBar { Indeterminate = true, ShowText = false };
new ProgressBar(() => 0.66f) { Vertical = true, Style = { Height = 90f } };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Value` | `Func<float>` | `null` | Value from 0 to 1. |
| `Indeterminate` | `bool` | `false` | Runs a segment about 25% of the bar length on a 1.6-second cycle. |
| `ShowText` | `bool` | `true` | Shows value text. |
| `Format` | `Func<float,string>` | `null` (default: `NN%`) | Formats value text. |
| `BarHeight` | `float` | `16` | Bar thickness. |
| `Vertical` | `bool` | `false` | Fills from bottom to top. |
| `StretchFill` | `bool` | `false` | Stretches fill artwork into the filled area. When false, fill is clipped by percentage. |
| `TrackStyle` | `Style` (readonly) | - | Track style override. |
| `FillStyle` | `Style` (readonly) | - | Fill style override, for example a gradient. |

## Exact `Indeterminate` formula

```text
seg   = 0.25 * barLength
cycle = (ctx.Time * 0.625) % 1
p     = -seg + (barLength + seg) * cycle
```

The visible part is the intersection of `[p, p + seg]` with `[0, barLength]`. Nothing is drawn when the visible piece is under 1 pixel, preventing edge flicker. The cycle is exactly 1.6 seconds regardless of FPS because it uses `ctx.Time` rather than frame count.

## Styles

Slots: `progressbar/track` and `progressbar/fill`. Both support 9-slice sprites; a fill sprite is drawn inside the filled area rather than clipped.

## Methods

| Method | Returns | Description |
|---|---|---|
| `ProgressBar(Func<float> value = null)` (constructor) | - | Creates a bar with a value source. |
| `GetValue()` | `float` | Reads `Value` directly when supplied; otherwise returns the latest rendered value. The component is read-only and has no `SetValue()`. |
| `SetStyle(string path, string value)` | - | Supports named parts `track` (`TrackStyle`), `fill` (`FillStyle`), and `text` (`TextStyleOverride`), for example `SetStyle("fill.color", "#00FF00")`. An unprefixed path targets the root style. |

## Events

There are no value callbacks; the value is read passively every frame. Universal events are available:

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
| Style | Yes | `TrackStyle`, `FillStyle`, and theme slots. |
| Sprite frame | Yes | `progressbar/track` and `progressbar/fill` (9-slice). |
| Animation | Yes | Shared `Style.Animation` plus custom time-based `Indeterminate` animation. |
| Disabled | No | No disabled state of its own. |
| Hover fade | No | Non-interactive element. |


---

[Table of contents](../index_en.md) - Indicators | Previous: [MeterGroup](05_metergroup_en.md) | Next: [ProgressSpinner](07_progressspinner_en.md) | [Русский](06_progressbar_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
