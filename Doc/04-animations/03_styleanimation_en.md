![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — core `0.6.7` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Animations | Previous: [Animated](02_animated_en.md) | Next: [Label](../05-forms/01_label_en.md) | [Русский](03_styleanimation_ru.md)

---

# StyleAnimation

Attach animation **by string** directly in a style or theme, without an `Animated` wrapper. The
same command-range post-processing is used, but `UiElement.EmitStyled` invokes it automatically for
any element with `Style.Animation` set.

`RimUI.Animation.StyleAnimation` (static class)

## Example

```csharp
var tag = new Tag("String pulse") { Style = { Animation = "pulse 1.2 loop" } };
var combo = new Tag("Fade+Slide") { Style = { Animation = "fade-in 0.6; slide-in-top 0.6" } };
```

The same string works in theme JSON. An `"animation"` key in any slot, for example
`"button": { "animation": "pulse 1.2 loop" }`, applies to every element using that slot without
any additional code.

## Animation string format

`"name [duration] [loop] [delay:X] [param:X]"`. Separate tokens with spaces or commas and
separate multiple animations with `;`.

| Spec part | Description |
|---|---|
| First bare number | Duration in seconds (`AnimSpec.Duration`). |
| `loop` | Enable looping (`AnimSpec.Loop = true`). |
| `delay:X` | Start delay in seconds (`AnimSpec.Delay`). |
| `param:X` | Module-specific parameter overriding the module default (`AnimSpec.Param`). |
| Second bare number | Same as `param:X` when no key is provided. |

## Styles

Uses `Style.Animation`; see Style. Unknown module names and malformed tokens are silently ignored,
making theme files tolerant of mistakes.

## Methods

| Method | Returns | Description |
|---|---|---|
| `static Install()` | `void` | Idempotently enable style-driven animation by assigning `UiElement.StyleAnimationHook`. Called once during adapter initialization. |
| `static Parse(string spec)` | `List<AnimSpec>` | Parse an animation string; results are cached by the original string. |

## Events

Not applicable. `EmitStyled` invokes the hook automatically and exposes no callbacks.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| No-code attachment | Yes | Put the string in `Style.Animation` or theme JSON. |
| Multiple animations | Yes | Separate them with `;`. |
| Custom modules | Yes | Any name registered through `Animations.Register`. |
| Typo tolerance | Yes | Unknown tokens and module names are silently skipped. |


---

[Table of contents](../index_en.md) - Animations | Previous: [Animated](02_animated_en.md) | Next: [Label](../05-forms/01_label_en.md) | [Русский](03_styleanimation_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
