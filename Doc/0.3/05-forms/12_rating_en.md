![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.31` · mod `0.3.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Forms | Previous: [InputBase](11_inputbase_en.md) | Next: [Select&lt;T&gt;](../06-selection/01_select_en.md) | [Русский](12_rating_ru.md)

---

# Rating

A star rating: the score is set by clicking a star. The value is a **fraction** (`float`) rather
than a star index — that is how half stars and external scores like "4.2 out of 5" are expressed:
they need to be displayed, but not necessarily entered by clicking.

`RimUI.Components.Rating : UiElement`

## Example

```csharp
// The value stays yours: the delegate is read every frame, no copy to keep updated
new Rating { Value = () => settings.Score, OnChange = v => settings.Score = v };

new Rating { AllowHalf = true };                       // clicking a star's left half gives .5
new Rating { Stars = 10, StarSize = 14f };             // ten smaller stars
new Rating { ReadOnly = true, Value = () => 4.5f };    // display only, mouse ignored
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `Stars` | `int` | `5` | Number of stars, which is also the maximum value. |
| `Value` | `Func<float>` | `null` | Value source. `null` = the value lives in the component's ID store. |
| `OnChange` | `Action<float>` | `null` | Called when a score is picked. |
| `ReadOnly` | `bool` | `false` | Display only: the mouse is ignored, the appearance stays normal. |
| `Disabled` | `bool` | `false` | Disabled: the mouse is ignored and the stars are dimmed. |
| `AllowHalf` | `bool` | `false` | Half values: clicking a star's left half gives `.5`. |
| `Cancel` | `bool` | `true` | Clicking the current score again clears it (value `0`). |
| `StarSize` | `float` | `18` | Star size. |
| `StarGap` | `float` | `3` | Gap between stars. |

## Behaviour

**Hover preview.** While the mouse is over the row, the stars show the score a click would set,
not the current value. The hovered value is computed before drawing — otherwise the first stars
would be drawn with the old value and the last ones with the new.

**Clearing the score.** Without `Cancel` a score of one could never be cleared: there is nothing
to click to the left of the first star. Clicking the current value again yields `0`.

**The gap between stars** belongs to the star on its left, so a click exactly between two stars
registers instead of falling into a void.

**`ReadOnly` versus `Disabled`.** `ReadOnly` means "this score is someone else's, there is nothing
to change": the appearance stays normal. `Disabled` means "unavailable right now": the stars are
dimmed with `rating/star_disabled`. Both ignore the mouse.

## Icons

The stars come from the shared icon sheet: `Icons.StarFull`, `Icons.StarEmpty`, `Icons.StarHalf`
(cells 19-21). To change how they look, replace the sheet — the component itself needs no changes;
see the `Icons` page.

## Styles

The theme slots supply **colour only** (`Text.Color`): the stars are drawn as tinted icons, so the
remaining style fields in these slots have no effect.

| Slot | Purpose |
|---|---|
| `rating/star` | An unfilled star. |
| `rating/star_on` | A filled star. |
| `rating/star_hover` | A filled star during the hover preview. |
| `rating/star_disabled` | All stars when `Disabled`. |

## Methods

| Method | Returns | Description |
|---|---|---|
| `GetValue()` | `float` | The current score: the `Value` source if set, otherwise the cache from the last drawn frame. |
| `SetValue(float v)` | — | Set the score programmatically: the value is clamped to `0..Stars` and `OnChange` is raised. Takes effect on the next frame. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnChange` | `Action<float>` | the new score | A star was clicked (including clearing, which passes `0`). |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | The same event in generic form: the index is the zero-based star number, the value is the fractional score. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The cursor entered or left the bounds. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | The cursor is over the element — EVERY FRAME. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left / right click on the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | `Disabled` changed (the component overrides `EffectiveDisabled`). |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Styling | Partly | The slots supply star colours; the row's own background and border use the regular `Style`. |
| Sprite frame | No | The stars are sheet icons, not 9-slice. |
| Animation | Yes | The common `Style.Animation` mechanism. |
| Disabled | Yes | The `rating/star_disabled` slot; input is blocked. |
| Hover fade | No | The preview switches instantly: easing would make it harder to land on the intended star. |
| Cursor | Yes | A hand over the row while the component is interactive. |


---

[Table of contents](../index_en.md) - Forms | Previous: [InputBase](11_inputbase_en.md) | Next: [Select&lt;T&gt;](../06-selection/01_select_en.md) | [Русский](12_rating_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
