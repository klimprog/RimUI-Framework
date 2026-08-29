![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Indicators | Previous: [ProgressBar](06_progressbar_en.md) | Next: [CircularProgress](08_circularprogress_en.md) | [Русский](07_progressspinner_ru.md)

---

# ProgressSpinner

A rotating ring used as a loading indicator. Size, speed, and color are configurable.

`RimUI.Components.ProgressSpinner : UiElement`

## Example

```csharp
new ProgressSpinner();
new ProgressSpinner { Size = 20f, Speed = 540f, Tint = new ColorRGBA(0.70f, 0.54f, 0.18f, 1f) };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Speed` | `float` | `270` | Rotation speed in degrees per second. |
| `Tint` | `ColorRGBA?` | `null` (from `spinner` or theme accent) | Ring color. |
| `Size` | `float` | `32` | Size, overridden by `Style.Width` when set. |

## Exact rotation formula

```text
Rotation = (ctx.Time * Speed) % 360   // degrees, clockwise
```

`Speed` is degrees per second, not radians. At `270`, one turn takes about `1.33` seconds. Rotation is clockwise; use a negative `Speed` for counterclockwise rotation.

## Styles

The `spinner` slot supplies only `Background.Color`. Its sprite is not used.

## Methods

Only the parameterless `ProgressSpinner()` constructor is public.

| Method | Returns | Description |
|---|---|---|
| `SetStyle(string path, string value)` | - | Applies the path to the root style; there are no named style parts. |

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
| Style | Partly | `Style.Width` overrides `Size`. |
| Sprite frame | No | Uses a fixed icon source; `spinner` does not read `Sprite`. |
| Animation | Yes (custom) | Time-based `Icon.Rotation`; shared `Style.Animation` remains available. |
| Disabled | No | Non-interactive element. |
| Hover fade | No | No smooth hover transition. |


---

[Table of contents](../index_en.md) - Indicators | Previous: [ProgressBar](06_progressbar_en.md) | Next: [CircularProgress](08_circularprogress_en.md) | [Русский](07_progressspinner_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
